# Mpenv安装配置及卸载(@SJTU'Pi)

>本笔记基于Mpenv的[sjtu分支](https://github.com/materialsproject/MPenv/tree/sjtu)，仅适合用于在sjtu的Pi上进行MPenv的安装配置。  

　　MPenv是M[aterials Project](https://materialsproject.org/)(G[ithub)](https://github.com/materialsproject)(后简称MP)中的设置的虚拟环境，配置该环境能够使得FireWorks更方便的完成自动化。MPenv的具体配置是，一套安装在自己账户目录中的Python2.7环境，并在环境上安装好了MP的主要软件如：p[ymatgen](http://pymatgen.org)(G[ithub)](http://pymatgen.org), c[ustodian](http://pythonhosted.org/custodian/)(G[ithub)](https://github.com/materialsproject/custodian), F[ireWorks](https://pythonhosted.org/FireWorks/)(G[ithub)](https://github.com/materialsproject/fireworks), MPWorks(G[ithub)](https://github.com/materialsproject/MPWorks)等。  
　　本笔记的内容仅包含MPenv(sjtu分支)的安装配置，MP的其他软件的使用请参考相应网站。


## 第一部分 安装管理员环境(admin_env)

>注意：一个账户(也就是一个用户名下)只需新建一次管理员环境，在该用户名下，可以新建很多个人环境。安装前请咨询hongzhu老师，来确认你的账户下是否已经安装完成管理员环境。如果已经安装完毕，请直接进行第二部分，向hongzhu老师申请环境名。

### 1. ssh登陆Pi
　　设定免密码登录请参照Pi文档[Log in The HPC Cluster via SSH](http://pi.sjtu.edu.cn/doc/ssh/)。
### 2. 修改.bashrc

```sh
vim ~/.bashrc
```
　　建议改为：

```sh
# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi

if [ -e $HOME/.bashrc.ext ]; then
	. $HOME/.bashrc.ext
fi

# .bashrc
source /lustre/utility/lsf/conf/profile.lsf
export MODULEPATH=/lustre/utility/modulefiles:$MODULEPATH
module load mkl/default
module load icc/default
module load gcc/default
module load impi/default

# User specific aliases and functions
```

　　其余可以自己酌情添加，icc和gcc的顺序不要动。保存后重新载入`~/.bashrc`

```sh
source ~/.bashrc
```

### 3. 加载必要modules

```sh
unset MODULEPATH;
module use /lustre/usr/modulefiles/pi;
module load anaconda/2
```

### 4. 建立管理员虚拟环境并下载安装所需文件
#### 4.1 建立管理员虚拟环境(admin_env)

```sh
cd ~;
mkdir ~/admin_env;
conda create --name admin_env numpy scipy matplotlib
```
　　激活管理员环境

```sh
source activate admin_env
```
#### ４.2 克隆Mpenv的sjtu分支

```sh
cd admin_env;
git clone https://github.com/materialsproject/MPenv.git;
cd MPenv;
git checkout sjtu;
git pull
```
### ５. 安装Mpenv

　　安装Mpenv:

```sh
cd ~/admin_env/MPenv/;
python setup.py develop
```

>注意！！不要直接运行`python ~/admin_env/MPenv/setup.py develop`，这样检测MPenv依旧是安装成功的，但是会导致以后环境路径出问题。

　　检查MPenv是否安装成功

```sh
which mpenv
```
　　如果出现`~/.conda/envs/admin_env/bin/mpenv`说明安装成功。

## 第二部分 在Pi上安装自己的虚拟环境
### 1. 申请自己的环境名称

　　向hongzhu老师申请你个人的环境名称。

### 2. 上传并解压压缩包

　　当你收到名为“环境名_files.tar.gz”的压缩包，即可在本地通过`scp`命令将其传至你在Pi的`$HOME`目录，(默认下载到了`~/Downloads`)。

```sh
scp ~/Downloads/压缩包名 Pi用户名@Pi主机Host:~
```
　　登陆Pi，解压：

```sh
tar -xvzf 环境名_files.tar.gz
```
### 3. 加载必要modules并激活管理员环境(admin_env)

```sh
unset MODULEPATH;
module use /lustre/usr/modulefiles/pi;
module load anaconda/2;
source activate admin_env
```
### 4. 安装个人的环境

>注意1:  
>环境名与压缩包名称相对应，压缩包名为“环境名_files.tar.gz”。  
>注意2:  
>如果你的用户的`$HOME`下没有 `.bashrc.ext`, 那么就在该处新建`.bashrc.ext`空文件。方法为：

>```
＃建立~/.bashrc.ext空文件
＞　~/.bashrc.ext
```

```
mpenv --conda --https 环境名
```
>注意3:  
>安装期间，因各种原因终止，则删除`~/环境名`文件夹，并清空`~/.bashrc.ext`中你环境名所对应的内容，并重新尝试。清空的方式，与建立空文件方式相同。

### 5. 修改源码以匹配pi的设定
#### 5.1 修改`~/.bashrc.ext`
```
vim ~/.bashrc.ext
```
　　修改对应行：

```sh
alias use_环境名='source activate ~/环境名/virtenv_环境名;后面不改动'
alias use_none='source deactivate;后面不改动'
```
　　退出保存后

```sh
source ~/.bashrc
```
　　测试：使用`use_环境名`进入虚拟环境，`use_none`退出虚拟环境。

#### 5.2 修改`my_qadapter.yaml`

```sh
vim ~/环境名/config/config_SjtuPi/my_qadapter.yaml
```
　　删掉`my_qadapter.yaml`中rocket_launch一行中的`--offline`，即修改为：

```sh
rocket_launch: rlaunch -c /lustre/home/账户名/环境名/config/config_SjtuPi singleshot
```

### 5.3 修改`wf_settings.py`

```sh
vim ~/环境名/codes/MPWorks/mpworks/workflows/wf_settings.py
```
　　将对应行修改为

```python
QA_VASP = {'nnodes': 2}
QA_VASP_SMALL = {'nnodes': 2, 'walltime': '24:00:00'}  # small walltime jobs
```

## 第三部分

### 1. 复制VASP\_PSP文件夹、VASP二进制文件:

```sh
scp -r umjzhh@202.120.58.229:~/VASP_PSP ~/software/VASP
mkdir -p ~/software/VASP/bin
scp -r umjzhh@202.120.58.229:~/keliu/VASP_unpacked/vasp.5.4.1.05Feb16/bin/ ~/software/VASP/bin
```
>以后编译版本可以放到`~/software/VASP/src/`，`~/software/VASP/bin`中的版本不要用软链接，这样在计算的时候同时编译新的VASP

### 2. 复制BoltzTraP二进制文件

```sh
mkdir -p ~/bin
scp -r umjzhh@202.120.58.229:~/bin/ ~/bin

```

### 3. 完善相关环境变量

　　修改`~/.bashrc.ext`

```sh
vim ~/.bashrc.ext
```
　　把`# for boltztrap`上面一行修改为：

```sh
export PATH=$HOME/software/VASP/bin:$PATH # for VASP　　
```
　　在个人环境最后(`# MPenv 环境名 end--->`之前)，修改VASP_PSP路径，加上个人的materials porject的api。

```sh
export PATH=$HOME/software/VASP/bin:$PATH # for VASP
export VASP_PSP_DIR=$HOME/VASP_PSP
export MAPI_KEY="你的MPAPI"
```

　　每次修改完`.bashrc.ext`,都因重新载入一下，让设置生效：

```sh
source  ~/.bashrc
```

#更新&修改
##更新管理员环境(尽量别用)
```sh
source activate admin_env
conda upgrade --all
```
>这个是原README中没提及的，但涉及Python版本这种程度的升级，或许会用到。然而这个命令用完了，package十有八九是降级的，所以管理员环境升级了，建议务必再进入个人环境升级一次。

```sh
use_none
# 退出管理员环境
```
##更新个人环境

```
update_codes
```
##修改Package
>谁叫咱用的是开发者模式，哈哈哈！   
>呵呵，也不怕闪了腰

　　修改相应package之后，定位到软件最上层目录(`~/环境名/codes/`的下一层)，运行：

```
python steup.py develop
```

#卸载or删除

　　如个人环境需要重装，只需删除环境文件夹，并清空`~/.bashrc.ext`个人设置即可。如管理员环境应该重装，应再删除`admin_env``.conda`。

## 卸载个人环境

　　清除`~/.bashrc.ext`中`# <---MPenv 环境名`到`# MPenv 环境名 end--->`之间的内容，删除`~/环境名`的文件夹，以只有一个个人环境为例，可以运行：

```sh
cp ~/.bashrc.ext{,.bak};
> ~/.bashrc.ext;
rm -rf ~/环境名;
```

## 卸载管理员环境

　　在删除个人环境的基础之上，再删除`admin_env`,`.conda`。

```sh
rm -rf ~/admin_env ~/.conda
```
