## 基于Ruoyi的SpringBoot单体应用docker-compose模板

### 项目描述

由于若依的ruoyi-vue项目没有docker部署模板，所以我按照ruoyi-cloud的格式配置了一个**适用于SpringBoot单体应用的通用docker-compose模板**。

该项目可以帮助你仅修改少量配置将**Mysql数据库**、**Redis缓存**、**Nginx反向代理**、**Java后端jar包**、**前端dist打包产物**通过脚本一键部署，炒鸡方便快捷。👍

### 使用说明

该模板适用于基于SpringBoot、Mysql、Redis开发的**单体应用**，烂大街的**基于Spring+Vue实现的XXX管理系统**等学生作业或毕设都可以通过该模板快速部署项目。

如果想用该模板部署自己的单体应用，你只需要遵循以下步骤：

1、将你数据库导出的sql文件放到`mysql/db`文件夹下。

2、打开`ngxin/conf/nginx.conf`，将`location /prod-api`的/prod-api/改成你请求接口的前缀，比如你的baseURL为api，则修改为location /api/。

3、把你通过maven打包的jar包放到`ruoyi/jar`文件夹下。（注意，需要将jar包重命名为**ruoyi-admin.jar**，若不想重命名可以打开`ruoyi/dockerfile`，将`COPY ./jar/ruoyi-admin.jar`的ruoyi-admin改成你的jar包名称）

4、把你前端项目的打包产物目录（一般为dist）替换`nginx/html`的dist目录。

5、以上步骤完成后，把整个目录上传到你的服务器，执行`./deploy.sh`即可一键部署。

### 注意事项

- 确保你的服务器已经安装docker，通过**docker -v**命令查看docker版本

- 如果deploy.sh无法执行，请检查是否有执行权限，可通过**chmod +x deploy.sh**为脚本放开执行权限

- 前端运行默认端口为80，后端运行默认端口为8080，如果修改端口号需将**nginx.conf**和**deploy.sh**的端口号相应地修改

- 项目所有文件/目录的命名都统一以ruoyi为前缀，如果想改成自己的命名建议全局搜索ruoyi再按需替换

- MySql和Redis的连接（比如账号密码）需要在后端一般为**application.yaml**文件配置，所以修改前请检查一致性

- 如果想要部署微服务项目，即扩展SpringCloud、Nacos等微服务中间件，可以参考Ruoyi-Cloud的docker-compose配置。[参考地址](https://github.com/yangzongzhuan/RuoYi-Cloud/tree/master/docker)

### 项目结构

```
ruoyi-docker-compose-template
├─ deploy.sh                    // 一键部署脚本
├─ docker-compose.yml           // docker-compose编排配置
├─ mysql                        // mysql镜像相关目录
│  ├─ db                        // mysql数据相关目录
│  │  ├─ readme.txt             // mysql文件配置说明
│  │  └─ ry-vue.sql             // mysql导出的sql文件
│  └─ dockerfile                // mysql镜像配置
├─ nginx                        // nginx镜像相关目录
│  ├─ conf                      // nginx配置相关目录
│  │  └─ nginx.conf             // nginx配置文件
│  ├─ dockerfile                // nginx镜像配置
│  └─ html                      // nginx存放的前端项目目录
│     └─ dist                   // 存放前端项目打包产物目录
│        └─ index.html          // 前端项目打包产物入口文件
├─ README.md                    // 项目文档
├─ redis                        // redis镜像相关目录
│  ├─ conf                      // redis配置相关目录
│  │  └─ redis.conf             // redis配置文件
│  └─ dockerfile                // redis镜像配置
└─ ruoyi                        // jar包相关目录
   ├─ dockerfile                // jar包镜像配置
   └─ jar                       // 存放jar包的目录
      └─ readme.txt             // 存放jar包的说明
```