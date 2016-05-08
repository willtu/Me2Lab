# SLURM任务的提交与追踪
　　本部分内容的基础为FireWorks中的`qlaunch`,`rlaunch`,及SLURM相关命令，建议阅读F[ireWorks](https://pythonhosted.org/FireWorks/)(G[ithub)](https://github.com/materialsproject/fireworks), MPWorks(G[ithub)](https://github.com/materialsproject/MPWorks)相关内容，以及SLURM相关文档P[i doc](http://pi.sjtu.edu.cn/doc/slurm/),O[ffice doc](http://slurm.schedmd.com/documentation.html)
## 任务提交
　　在π上一般利用`qlaunch`实现自动提交。然而终端关闭后，`qlaunch`的提交也会随之终止，故加上`nohup`来进行持续提交任务，最终实现方案如下：

```sh
alias qlaunch = 'if [ $( pgrep -f qlaunch | wc -l ) -eq 0 ]; then  mkdir -p ./log;  nohup qlaunch -r rapidfire --nlaunches infinite -m 6 --sleep 30 -b 10000 > ./log/qlaunch.out & fi'
```
>当前，π允许每账户同时运行最大任务数为6，亦即-m(which means max)参数的设置。

　　在`~/.bashrc.ext`中设置此`alias`，以后执行`qlaunch`便会判断是否存在`qlaunch`进程，当进程不存在则持续提交任务，并将提交日志保存在当前位置的log目录下。  
　　另外，查看用户所有任务，和终止提交可以采用：

```sh
# 查看
ps -u 用户名
# 终止提交(qlaunch)
pkill -f qlaunch
```
## 任务追踪
　　SLURM系统的工作过程，任务先提交到队列(squeue)中，然后系统根据你的计算要求，给你分配节点进行计算。

```sh
# 当前队列状况：
squeue
# 当前账户历史任务：
sacct -u 账户名
# 已知 SLURM job_id (在FireWorks中对应qid) 查看某任务详情：
sacct -j 任务序号
# 图形化界面(Mac需安装xquartz)，并ssh -Y 登录
sview -i & 2
```
>Mac安装xquartz的方法为：`brew cask instal xquartz`

　　相应，追踪任务的主要目的，宏观上是查看这批任务的整体状况，具体则是解决运行出现问题的任务，于是定位问题任务，在FireWorks中有相应的命令：

```sh
# FireWorks报告
lpad report
# 某状态(FIZZLED,RESERVED,COMPLETED,RUNNING等)firework总览
lpad get_fws -s 状态
# 某状态(FIZZLED,RESERVED,COMPLETED,RUNNING等)workflow总览
lpad get_wflows -s 状态
# 通过fw_id(FireWorks id)获取任务详情
lpad get_fws -i fw_id
# 通过qid(SLURM job_id)获取任务详情
lpad get_fws --qid qid
```
　　虽然，任务详情和工作流详情都可以通过加`-d more`的参数来获得，但是并不建议这样做，因为出来的结果，其实就是MongoDB对应任务全部保存下来写成了一个json文件，不如直接上MongoDB上搜索。  
　　MongoDB搜索方式，稍后补全。  
　　当定位到问题任务的launcher目录，就可以通过目录中的`.error`及其他文件来分析出错的原因并采取相应的更改。

##建议
　　FireWorks的文件结构是，一个`block`文件夹下有很多`laucnher`文件夹，每个`laucnher`文件夹都对应着一个firework。
>但是，并不是每个firework都对应着一个`launcher`，而是至少一个，每次运行就会生成一个`launcher`，于是任务失败了几次，就多了多少个`launcher`。

　　而且，这些`launcher`目录的命名是根据时间的(并且π的设定还是有8小时时差的！)，看一屏幕`launcher`的感觉就是悠长而凌乱。因此拥有好的目录结构是非常有必要的。  
　　生成`block`文件的位置就是`qlaunch`命令执行的位置，`-b`参数设定多少个`launcher`换一个`block`目录。如果不喜欢目录漫天乱飞的话，建议每次都在固定的位置运行`qlaunch`命令都在固定的位置。  
　　个人习惯是这样的：  
　　脚本写在一个目录`~/script`(仍有子目录区分)，`launcher`单独放一个目录`~/launcher`,每一批运算(对应一个脚本)设定好`launcher`下的子目录，把脚本软链接(`ln -s`)过来，然后在这个子目录下，提交，并`qlaunch`。这样的好处是，脚本与`qlaunch`互补干扰，每批运算也不会有过多`block`目录，文件结构比较清爽。