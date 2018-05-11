```
docker image pull library/hello-world
```

上面代码中，docker image pull是抓取 image 文件的命令。library/hello-world是 image 文件在仓库里面的位置，其中library是 image 文件所在的组，hello-world是 image 文件的名字。

由于 Docker 官方提供的 image 文件，都放在library组里面，所以它的是默认组，可以省略。因此，上面的命令可以写成下面这样。
```docker image pull hello-world```

```
docker image ls

# 从 image 文件，生成一个正在运行的容器实例
docker container run hello-world

docker container kill [containID]

docker container start [containID]
docker container stop [containID]

docker container ls

docker container ls --all

docker container rm [containID]

```

1. 新建 .dockerignore
```
.git
node_modules
npm-debug.log
```

2. 新建 Dockerfile
```
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000

```
代码释义

```
FROM node:8.4：该 image 文件继承官方的 node image，冒号表示标签，这里标签是8.4，即8.4版本的 node。
COPY . /app：将当前目录下的所有文件（除了.dockerignore排除的路径），都拷贝进入 image 文件的/app目录。
WORKDIR /app：指定接下来的工作路径为/app。
RUN npm install：在/app目录下，运行npm install命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
EXPOSE 3000：将容器 3000 端口暴露出来， 允许外部连接这个端口。
```

3. 创建 image
```
docker image build -t koa-demo .

或者
# -t 指定image的名字，: 指定标签，默认标签是 latest，. 指定上下文路径
docker image build -t koa-demo:0.1 .

那么为什么会有人误以为 . 是指定 Dockerfile 所在目录呢？这是因为在默认情况下，如果不额外指定 Dockerfile 的话，会将上下文目录下的名为 Dockerfile 的文件作为 Dockerfile。

这只是默认行为，实际上 Dockerfile 的文件名并不要求必须为 Dockerfile，而且并不要求必须位于上下文目录中，比如可以用 -f ../Dockerfile.php 参数指定某个文件作为 Dockerfile。

当然，一般大家习惯性的会使用默认的文件名 Dockerfile，以及会将其置于镜像构建上下文目录中
```


4. 其他命令

```
docker container logs [containID]

docker container exec

docker container cp [containID]:[/path/to/file] .
```
