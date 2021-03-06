[![](https://images.microbadger.com/badges/image/qq58945591/walle-web.svg)](http://microbadger.com/images/qq58945591/walle-web "Get your own image badge on microbadger.com")[![Docker Automated buil](https://img.shields.io/docker/automated/jrottenberg/ffmpeg.svg)](https://hub.docker.com/r/qq58945591/walle-web/)

# 关于本镜像:

#### 集成打包lnmp环境的walle-web部署工具,适合懒人们使用. 根据php docker官方Dockerfile配置修改,有问题可以反馈给我.

#### 环境配置是:
PHP 5.6.23
Nginx 1.10.1
MariaDB 10.1.14

#### 系统组件:
git nginx mysql gettext libmcrypt-dev icu-libs zlib-dev unzip subversion ansible openssh-client

#### php组件:
bcmath intl mbstring mcrypt mysqli opcache pdo_mysql


### 如何使用?
1. 简单运行命令example.  

```
docker run -d --name walle-web -v /data/mysql:/opt/mysql -p 80:80 qq5894591/walle-web

```

(注意:－v 是挂载一个本地目录到容器内做mysql的永久数据保存，否则在容器被销毁的时候，数据将丢失.冒号前面的路径是本地主机的目录,后面是容器的目录。修改本地主机的即可.另外可加参数`--restart always` 让容器在本地主机重启后会自动启动,当walle-web 有更新的时候,重新pull镜像,运行就可以了,数据都是保存在本地指定的数据库目录.)

2. 可以使用walle自身支持的各种变量来改变默认的walle配置,因为walle默认使用root作为登录用户, 可以使用变量WALLE_DB_PASS设置root密码,如未设置的话，默认密码是walle:

```
docker run -d --name walle-web -v /data/mysql:/opt/mysql -p 80:80 -e WALLE_DB_PASS="you_passwd" qq5894591/walle-web

```

3. 可以使用变量`-e SERVER_NAME=www.example.com` 设定虚拟主机名，未设置则默认使用`walle.company.com`如:

```
docker run -d --name walle-web -v /data/mysql:/opt/mysql -p 80:80 -e SERVER_NAME="www.example.com" qq5894591/walle-web

```

4. 本镜像支持设定walle 需要的一些可选变量设定,使用docker run时增加`-e var="value"`参数即可:

```
#设定walle使用的域名.
SERVER_NAME

#设定mysql root密码,也是walle数据库密码.
WALLE_DB_PASS

#设定walle的smtp功能.
WALLE_MAIL_HOST
WALLE_MAIL_USER
WALLE_MAIL_PASS
WALLE_MAIL_PORT
WALLE_MAIL_ENCRYPTION

```

5. walle默认自动开启ldap功能,导致了访问失败,本镜像禁用了ldap功能.如果需要使用ldap功能,请自行修改配置文件:

在walle目录下找到配置文件config/params.php

修改配置前

```
'user_driver'=>'',

```

启用ldap,其他ldap设置在下面一并可以设置.

```
'user_driver'=>'ldap',

```

