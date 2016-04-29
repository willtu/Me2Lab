#基于Julia实现MP数据库的稳定性分析

## 1. 配置Julia环境
### 1.1 安装Julia

```sh
brew install julia mongo-c
```

### 1.2 安装Mongo.jl,YAML.jl

Mongo对mongo-c有依赖，但是编译顺序不太对，首先安装mongo-c可以解决编译问题。

```sh
brew install mongo-c
```

然后进入`julia`

```julia
Pkg.update()
Pkg.add("Mongo")
Pkg.add("YAML")
```