
学习使用.

nginx 负载tomcat .

创建自定义网络test 

`docker network create --subnet=172.88.0.0/16 test`

网络名`test`和`ip`会在`docker-compose`中使用,用来指定网络和IP,同时IP与nginx负载配置

目录结构
```
├── docker-compose.yml
├── etc
│   └── localtime
├── nginx
│   ├── Dockerfile
│   ├── nginx-1.12.2.tar.gz
│   └── nginx.conf
├── tomcat
│   ├── apache-tomcat-8.5.24.tar.gz
│   ├── Dockerfile
│   └── jdk-8u45-linux-x64.tar.gz
└── webserver
    ├── tomcatA
    │   └── index.jsp
    └── tomcatB
        └── index.jsp
```
