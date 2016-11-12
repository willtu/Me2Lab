# Matmethods安装配置(@SJTU'Pi)

参考:项[目主页](http://pythonhosted.org/MatMethods)(G[ithub](https://github.com/hackingmaterials/MatMethods))。相关软件访问M[aterials Project项目组](https://github.com/materialsproject)。

## 0. 准备工作

环境变量设置

`~/.shrc`的建议设置

```sh
# ~/.shrc
# Source global definitions
if [ -f /etc/shrc ]; then
	. /etc/shrc
fi

# global proxy settings
# export http_proxy=http://mu04:3004/
# export https_proxy=http://mu04:3004/

# Modules
unset MODULEPATH
module use /lustre/usr/modulefiles/pi
# you'd better load particular version instead of 'default'
module load anaconda/2
module load mkl/11.2
module load icc/15.0
module load gcc/4.9
module load impi/5.0
```
创建安装目录

> 我的实际安装目录是/lustre/home/umjzhh-1/matmethods，codes是放repo的位置，直接创建出来省点事情，但是config中很多路径都需要填写full path。自己酌情填写。

```sh
mkdir -p ~/matmethods/codes
```

## 1. 建立python虚拟环境

创建python虚拟环境

> 此处建了一个python3的环境，发现python3比python2是有优势的。

```sh
conda create -p ~/matmethods/env python=3 numpy scipy matplotlib
```

克隆Materials Project的软件:

```sh
cd ~/matmethods/codes;
git clone https://www.github.com/materialsproject/fireworks.git;
git clone https://www.github.com/materialsproject/pymatgen.git;
git clone https://www.github.com/materialsproject/pymatgen-db.git;
git clone https://www.github.com/materialsproject/custodian.git;
git clone https://github.com/hackingmaterials/MatMethods.git matmethods
```

在Develop模式安装Materials Project的软件们:

> 下述代码pymatgen装了2遍，因为其他软件对pymatgen有依赖，按照默认`ls`顺序会装上pymatgen的pip版本。当然也可以手动在每个目录都`python setup.py develop`。

```sh
source activate /lustre/home/umjzhh-1/matmethods/env;
cd ~/matmethods/codes/pymatgen;
python setup.py develop;
cd ..
for i in $(ls); do
	cd $i
	python setup.py develop
	cd ..
done
```

装完后检查上述5个软件是否装成，及路径：

```sh
pip list
```
如果上述软件的路径都在`~/matmethods/codes`，说明安装成功，且模式正确(develop)。

## 修改配置文件

先把模板复制过来

```sh
cd ~/matmethods;
cp -r codes/matmethods/vasp/config_example/standard_config config
```
修改，还是看主页吧: http://pythonhosted.org/MatMethods/

配置进入环境变量:

```sh
vim ~/.bashrc.matmethods
```

```sh
# ~/.bashrc.matmethods
source activate /lustre/home/umjzhh-1/matmethods/env
export FW_CONFIG_FILE=/lustre/home/umjzhh-1/matmethods/config/FW_config.yaml

alias update_codes='
    wd=$(pwd);
    cd ~/matmethods/codes;
    for i in $(ls ~/matmethods/codes); do
        cd $i
        git pull
        python setup.py develop
        cd ..
    done;
    cd $wd
'

alias use_none='source deactivate; export FW_CONFIG_FILE="";'
```

然后，在`~/.bashrc`中添加:

```sh
alias use_matmethods='source ~/.bashrc.matmethods'
```
