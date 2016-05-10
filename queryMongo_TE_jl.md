#基于Julia实现MP数据库的稳定性分析

## 1. 配置Julia环境
### 1.1 安装Julia

```sh
brew install julia mongo-c
```

### 1.2 安装Mongo,YAML,PyCall,PyPlot

Mongo对mongo-c有依赖，但是编译顺序不太对，首先安装mongo-c可以解决编译问题。

```sh
brew install mongo-c
```

然后进入`julia`

```julia
#设置julia内python环境变量
ENV["PYTHON"] = "/usr/local/bin/python"
Pkg.update()
Pkg.add("Mongo")
Pkg.add("YAML")
Pkg.add("PyCall")
Pkg.add("PyPlot")
```