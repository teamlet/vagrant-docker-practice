# docker 技术

## docker 官网

* https://www.docker.com/

## docker安装mediawiki

* 1、安装 mysql docker

```
    docker pull mysql:5.7
 
    docker run -itd -p 3306:3306 --name wiki-mysql -e MYSQL_ROOT_PASSWORD=123456 
                --restart=always --restart=on-failure:1 --oom-score-adj -1000 
                --privileged=true --log-opt max-size=10m --log-opt max-file=1 mysql:5.7
```
 
 * 2、安装 mediawiki docker
 
```
 docker pull mediawiki:1.34
 
 docker run -itd --name mywiki -p 80:80 --privileged=true --restart=always
     
                --link wiki-mysql:mysql mediawiki:1.34
```
 
 * 3.设置数据库
 
 在主机端使用mysql client(使用pyCharm DB Explore)连接mysql数据:
 
```
 IP：192.168.1.22
 
 用户名:root
 
 密码：123456
```

 执行下面语句：
 
```
 mysql> create database wikidb;

 mysql> create user 'wikiuser'@'192.168.1.%' identified by '123456';

 mysql> grant all privileges on wikidb.* to 'wikiuser'@'192.168.1.%' with grant option;

 mysql> flush privileges;
```

* 4.设置 wiki

    通过 http://192.168.1.22 访问wiki，进行安装。
    
    在设置数据库的时候，选择 mysql; 
    
    IP地址必须使用 192.168.1.22，如果使用localhost或者 127.0.0.1无法访问数据库。
    
    设置完成后，会自动下载一个配置文件 LocalSettings.php，把这个文件复制到当前运行的虚拟机 vagrantfile 所在文件夹中。
    
    在 192.168.1.22 虚拟机中，执行：
    
    `docker cp /vagrant/LocalSettings.php mywiki:/var/www/html`
    
    然后执行:
    
    `docker exec -it 5a7ea412f57c /bin/bash`
    
    这里的 5a7ea412f57c 是 mywiki 的Container ID，使用 `docker ps -a ` 查看。
    
    上面的命令执行完成后，进入mywiki container中，使用 `ls -l` 可以查看复制进来的 LocalSettings.php
    
    在wiki安装界面，进入首页，完成安装。
    