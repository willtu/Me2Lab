#Materials Project

## 1 FireWorks+MPWorks 
　　在FireWorks的理念里，每步VASP计算(job)都被称为一个firework，此外将job提交到SLURM，将数据插入数据库，都会被称为一个firework。材料从pymatgen生成输入文件，到最终计算优化完成这样的过程，就是由一个个的firework串起来的，整个这个过程称之为workflow。  
　　FireWorks的工作就是管理firework和workflow，大部分功能，可以看`lpad -h`，此处不详述。  
　　MPWorks现在定义了热电材料计算的workflow，当然，以后会兼容更多的材料。

## 2 Pymatgen
　　生成(包括输入以及编辑)DFT软件所需的输入文件，或生成SNL(StructureNL)变量直接提交。  
　　其次，根据材料结构绘制相关图谱。

## 3 cutodian
　　在计算任务过程中，检测错误，终止计算，并修改参数重新提交计算。