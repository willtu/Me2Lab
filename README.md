# Materials Project在SLURM(@π)的使用

　　以MP(Materials Project)为主体，在SLURM任务调度系统上使用的第一性原理高通量计算的实现，包含三个方面的内容：调度系统SLURM、数据库MongoDB、以及函数库Materials Project的软件们。  
　　从更加微观的层面上看，MP本身并不具有计算能力，它通过调用登录节点的VASP，BoltzTraP等软件实现计算。然而，MP使人们不必再直接面对VASP等软件，也就使他们的存在渐渐淡化。

## SLURM

　　SLURM是一个任务调度系统，用以管理、分配任务计算的资源(主要指CPU、GPU)。在SLURM系统中，利用本人**账户中的文件、程序(也可以module load π上编译好的程序)**，以及**非登录节点的CPU(以及GPU)**来完成计算。  
　　SLURM和MP(Materials Project)软件的连接，主要通过FireWorks中的qlaunch和rlaunch来完成。

* [SLURM中任务的提交与追踪](submit_track.md)

## MongoDB

　　MongoDB是MP计算时，保存计算结果的数据库。它使得后续分析的时候，调用更加方便。
　　
## Materials Project
　　个人账户中都构建了MPenv环境，其中包括Pymatgen、custodian、FireWorks、MPWorks等。其主要的作用如下：  

* Pymatgen:生成DFT计算的输入文件、简单处理数据；
* custodian:在DFT计算过程中发现并纠正错误；
* FireWorks:管理任务(firework)、工作流(workflow);
* MPworks:定义热电材料计算工作流;
* MPenv:安装配置MPenv;
* Pymatgen-db:暂时不知道。


　　
