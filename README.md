## 配置要求
- Docker
- Docker-compose

我使用的是Mac电脑，Docker新推出了Docker for Mac

Docker for Mac整合了docker-machine , docker-compose
如需下载请[点击此处](https://download.docker.com/mac/stable/Docker.dmg)

## 配置 Docker for Mac

### 设置Docker CUP ， 内存 

安装之后可以更改docker虚拟机配置（内存充足的话，建议提高docker配置）

![](png/docker01.png)



### 设置Docker镜像，加速下载

这里我用的是aliyun提供的Docker镜像，速度还可以

![](png/docker02.png)


Clone本项目

```
cd ~/Work
git clone https://github.com/mmfei/docker_php_nginx_mysql_redis_memcached
cd docker_php_nginx_mysql_redis_memcached
```

## 一键完成(在进入clone出来的项目目录中执行)
```
mkdir -p ~/Work/data;mkdir -p ~/Work/htdocs/default;mkdir -p ~/Work/log/nginx;mkdir -p ~/Work/log/php;mkdir -p ~/Work/data/elasticsearch;mkdir -p ~/Work/elasticsearch/data;mkdir -p ~/Work/logstash/conf;echo '<?php phpinfo(); ?>' > ~/Work/htdocs/default/phpinfo.php;docker-compose build;docker-compose up -d;docker-compose ps;open http://localhost/phpinfo.php;
```

# 下面是对上面一键完成命令的步骤分部解释

## Docker-compose 使用


## 创建Work脚本
```
mkdir -p ~/Work/data;
mkdir -p ~/Work/htdocs/default;
mkdir -p ~/Work/log/nginx;
mkdir -p ~/Work/log/php;
mkdir -p ~/Work/data/elasticsearch;
mkdir -p ~/Work/elasticsearch/data;
mkdir -p ~/Work/logstash/conf;
```
## ~/Work 目录结构(只列出容器需要的目录 , 需要自己提前创建)
```
/Users/mmfei/Work
├── data
│   ├── mysql
│   │   ├── auto.cnf
│   │   ├── ib_logfile0
│   │   ├── ib_logfile1
│   │   ├── ibdata1
│   │   ├── mysql
│   │   └── performance_schema
│   └── redis
│       └── dump.rdb
├── htdocs
│   ├── adapi.mmfei.cn
│   ├── adnew.mmfei.cn
│   └── default
│       └── phpinfo.php
└── log
    ├── nginx
    │   ├── access.log
    │   └── error.log
    └── php
        ├── fpm-access.log
        └── fpm-error.log
```

docker-compose 构建docker服务

```
docker-compose build
```
此时会看到：

![](png/docker-compose.png)


全部构建成功之后使用docker-compose up -d 开启服务

```
docker-compose up -d
```

此时会看到：

![](png/docker-composeup.png)


## 测试 phpinfo()

```
echo '<?php phpinfo(); ?>' > ~/Work/htdocs/default/phpinfo.php
```

现在可以通过访问`http://127.0.0.1/phpinfo.php` 来查看phpinfo

![](png/phpinfo.png)


## 常用命令 
移除所有的容器和镜像（大扫除）
用一行命令大扫除：
```
docker kill $(docker ps -q) ; docker rm $(docker ps -a -q) ; docker rmi $(docker images -q -a) 
```

注：shell 中的 $() 和 `` 类似，会先执行这里面的内容，上面的脚本会出现如下 docker kill "pids" ; docker kill 在 docker 中用于停止容器，docker rm 删除容器， docker rmi 删除镜像
当没有运行的容器或者是根本没有容器的时候，这只会提示一个警告信息。当你想尝试的时候，这就是个非常好的单行命令。
如果你仅仅想删除所有的容器，你可以运行如下命令：
```
docker kill $(docker ps -q) ; docker rm $(docker ps -a -q) 
```

查看容器logs
```
docker-compose logs xxx -f
```

[点此查看更多常用命令](http://dockerlux.github.io/pdf/cheat-sheet-v2.pdf?spm=5176.100239.blogcont57839.60.kfpwer&file=cheat-sheet-v2.pdf)

