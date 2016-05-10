# Julia环境配置
## 1 安装Julia

```sh
brew install julia 
```

## 2 安装Pkg
　　Pkg就是Julia的函数包，julia是内置函数包管理器的。函数包的一般管理命令为：

```julia
Pkg.add("name_of_Pkg")	#安装Pkg
Pkg.rm("name_of_Pkg")	#卸载Pkg
Pkg.update()			#升级所有Pkg
Pkg.stats()				#列出安装包及依赖版本
```
　　一般来说，建议在安装之前运行`Pkg.update()`，再执行安装。
## 2.1 Mongo
　　Mongo对mongo-c有依赖，但是编译顺序不太对，首先安装mongo-c可以解决编译问题。

```sh
brew install mongo-c
```
　　然后再进入`julia`安装。
## 2.2 PyCall,PyPlot
　　安装PyCall之前需设置Python环境变量，在执行安装。

```julia
#设置julia内python环境变量
ENV["PYTHON"] = "/usr/local/bin/python"
Pkg.update()
Pkg.add("PyCall")
Pkg.add("PyPlot")
```
## 2.3 YAML
　　直接装。