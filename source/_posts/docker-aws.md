---
title: 将docker镜像部署到AWS并运行
date: 2016-11-17 14:16:01
tags:
- docker
- AWS
---

# 如何将docker镜像部署到AWS上并运行


`workflow`

1.利用sinatra启动本地的web server提供web服务

2.安装docker服务

3.将sinatra提供的websever安装到docker中

4.本地设置端口映射调试

5.注册AWS，部署docker镜像并运行


-----------------------------
## step1 利用sinatra启动本地的webserver提供web服务
> sinatra 的官网为：http://www.sinatrarb.com/

> sinatra 的github网址为：https://github.com/sinatra/sinatra

文档很详细，启动一个web 的服务只需要：

新建一个ruby的文件，命名为myapp.rb，代码如下
```
# myapp.rb
require 'sinatra'

get '/' do
  'Hello world!'
end
```

Install the gem:
```
gem install sinatra
```

And run with:
```
ruby myapp.rb
```

View at: http://localhost:4567 (默认的端口，可以更改)即可，会看到页面上出现`Hello world!`

Note: sinatra会提供路由的处理，本例只是一个最最简单的get方法的路由访问；
如果没有ruby和gem的命令，请参见google先安装ruby；

## step2 安装docker
本机使用osx系统，所以打开docker官网 https://docs.docker.com/docker-for-mac/  直接下载dmg文件安装即可，安装详情请参考此文档 https://docs.docker.com/docker-for-mac/#/step-1-install-and-run-docker-for-mac

## step3 将sinatra提供的websever安装到docker中
思路：如何实现这步，首先我们需要建立一个Dockerfile，用于将执行的命令写入；之后需要新建一个docker image即可.

a.新建一个Dockerfile文件
内容如下：
```
# This is a comment
FROM ubuntu:14.04
MAINTAINER sialvsic <sialvsic@outlook.com>
RUN mkdir /run-docker
WORKDIR /run-docker
RUN apt-get update && apt-get install -y ruby
RUN gem install bundler
COPY main.rb main.rb
COPY Gemfile Gemfile
COPY config.ru config.ru
RUN bundle install
EXPOSE 8081
CMD rackup --host 0.0.0.0 -p 8081
# CMD ["bundle", "exec", "rackup", "--host", "0.0.0.0", "-p", "8081"]
```

> `#` 注释

> EXPOSE 表示显示的声明要导出的端口号

> CMD 表示build时不运行，但是在容器执行时运行的命令

新建好Dockerfile之后，执行命令
```
docker build -t star:1.0 .  
```

-t 表示；

. 表示当前路径；

执行docker images 命令会看到存在一个REPOSITORY 为star TAG为1.0的一条新的镜像，此时镜像已经build成功

## step4 设置本地端口映射
我们清楚，docker 是运行在一个虚拟机中的，本质上也是一个小型的精简的linux系统，所以docker的镜像运行时只会在linux系统中打开一个8081端口，但是在当前的osx的系统中是没有办法访问的，所以需要配置端口映射，如何配置：仅需要在docker的镜像运行时指定-p参数，eg:
```
docker run -d -p 7080:8081 star:1.0
```
-d表示后台运行，7080代表hostport,8081代表docker内部的port号，
此时打开浏览器输入： localhost:7080 即可访问

## step5 注册AWS，部署docker镜像并运行
注册很简单，唯一注意的就是需要绑定信用卡

如何部署，Follow以下文档
https://aws.amazon.com/cn/getting-started/tutorials/deploy-docker-containers/

## 常见技巧
### 删除所有的docker containers
```
docker rm $(docker ps -a -q)
```

### 删除所有的docker images
```
docker rmi $(docker images -q)
```


## 常见问题
### 无法删除镜像
场景：使用docker rmi 命令时出现以下提示
Error response from daemon: Conflict, cannot delete 91c95931e552 because the container 76068d66b290 is using it, use -f to force FATA[0000] Error: failed to remove one or more images  

分析：因为镜像正在运行或者镜像间存在依赖

解决：加上-f 参数强制删除

### Don't run Bundler···
场景: docker build 时出现以下的提示
Don't run Bundler as root. Bundler can ask for sudo if it is needed, and
installing your bundle as root will break this application for all non-root
users on this machine.

分析：是一个错误警告，但不是一个错误，暂时还没有解决

解决：

### Could not locate Gemfile or .bundle/ directory
场景：运行docker build报错

分析：在书写Dockerfile的过程中，需要考虑到自己写的文件的位置，牵扯到运行问题。docker 提供COPY和ADD可以用于拷贝文件到docker虚拟机，但是存在docker的运行解析问题，可能会找不到文件

解决：使用 WORKDIR处理好

### 已经配置好了端口映射但是本地仍然无法访问
场景：已经建立好了镜像，在运行镜像时尝试执行了以下命令均无效果
```
docker run -d -p localhost:7080:8081 star:1.0
docker run -d -p 7080:8081 star:1.0
docker run -d -p 127.0.0.1:7080:8081 star:1.0
```

分析：看了文档觉得这些都是对的，一开始就很奇怪这样子为什么不对，早上无意间看了一下别人的问题试了一下解决了，但是还不是很清楚为什么加了这个参数就可以了。

解决：在Dockerfile的web运行时加入参数 --host 0.0.0.0
