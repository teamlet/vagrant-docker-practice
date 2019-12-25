# vagrant-docker-practice

# 虚拟环境安装记录



# 一、起因

通常使用 vmware 或者 virtualBox 在windows下安装 linux 。

使用过程有两个主要步骤：

1. 需要先下载一个linux的iso镜像，然后设置成 vmware的启动光盘；

2. 启动vmware后，开始安装linux系统。

安装好后，每个虚拟机占磁盘十几个G至几十个G，复制备份麻烦。



## 二、vagrant

vagrant是一个工具。

使用vagrant可以避免使用 vmware的两个步骤。

vagrant 提供直接运行的镜像iso，无需自己动手安装。

vagrant可以直接从vagrant的网站上，通过url或者名字下载iso镜像，这些iso镜像称为 box。可以避免自己到处寻找iso镜像，而且很多各种定制内容的镜像可以选择使用。

在运行iso之前，vagrant生成 vagrantfile 配置文件，可以通过配置文件增加定制的行为。

默认配置的 vagrant 运行虚拟环境没有virtualBox的GUI界面，感觉就像使用远程服务器一样。

根据vagrant运行过程产生的内容，可以分为两个部分：

1. 下载的镜像文件（每个镜像文件称为一个 box ）

   这些 box 保存到 vagrant_home目录boxes下,每个box都是一个文件夹，除了vmdk的镜像文件，还有一些配置和日志文件。（vagrant_home可以通过系统环境变量设置，设置的目的是避免使用默认的C 盘，下载box 需要占用大量的磁盘空间，很容易导致C 盘空间不足。）

2. vagrant使用 box 

   首先要执行 `vagrant init  [box-name] `,  box-name 就是在vagrant网站下载的镜像的名字。执行 init 命令后，会产生 vagrantfile 文件。

   

   只要有了box，并创建了vagrantfile，就可以启动虚拟机 `vagrant up`， vagrant会调用 virtual box 虚拟机应用程序，根据box-name，在vagrant_home的boxes中查找镜像文件，再根据vagrantfile的配置内容，创建虚拟运行环境。

   说了这么多，还没有体现vagranat的优势，为什么要用vagrant呢？

   
   vagrantfile是一个文本文件，类似 apache httpd.conf 或者nginx的配置文件。

   如果有一个svn版本管理系统，通过svn对vagrantfile进行管理和共享。

   开发成员只需要执行两个命令，就可以完全复制一个相同的开发环境：

   1. 从vagrant网站下载镜像 - `vagrant box add [box-name]` 
   2. 下载svn中的vagrantfile后，在vagrantfile文件所在目录执行 - `vagranta up`即可！

   另外，使用一个 box 镜像，可以创建任意多个独立的虚拟环境。
   比如，在尝试安装、配置软件过程中，出现不可恢复的错误，直接删除当前的虚拟环境，重新创建即可。

   如果使用的环境比较简单，这些事情 docker 也可以做。但是，如果环境复杂，特别是特殊的测试环境，vagrant有很大的优势。还有，vagrant在windows、linux、mac上都可以很好的兼容，而且也支持很多的虚拟软件，比如 vmware、virtualBox、Hyper-V,Docker

   

   docker在windows 10 Home版是不可以用的！

   vagranta可以在windows XP 下使用。如果想在windowsXP下玩docker，就可以借助vagrant了！

   

   vagrant 网络配置

   

   没有网络，虚拟环境就是孤岛，作用发挥不了。

   vagrant可以配置多种网络：

   

   1. 自己使用的网络配置，只有自己的主机和虚拟环境双向通讯

      * 使用默认IP 地址

        config.vm.network :forwarded_port, guest: 80, host: 4567 # 默认使用 127.0.0.1 IP

      * 使用指定IP地址

      ​       config.vm.network "private_network", ip: "192.168.33.10"  # 指定使用静态IP

   2. 局域网使用的网络配置

      * 使用默认分配的IP地址

        config.vm.network "public_network" # 分配的IP通常是 172 或者 10 网段 

      * 使用指定IP地址

        config.vm.network "public_network", ip: "192.168.1.22" # 指定IP，局域网可以访问

   

   

## 三、docker 

记录一些基本的使用。环境是在 windows 10 Home版本，安装了vagrant，下载 williamyeh/centos7-docker  安装，并配置了局域网可以访问的静态IP。

1. 显示版本

   docker -v 

2. 安装例子

   docker pull hello-world  # 安装 hello-world 镜像

   docker images                 # 显示镜像列表

   docker run -P -d hello-world  # 运行 hello-world

   docker ps  -a   # 查看执行状态

3. 安装 nginx

   docker pull daocloud.io/library/nginx  

   docker run -d -p  88:80 --name nginx-test daocloud.io/library/nginx  # 88是本地端口 80 是 nginx 运行端口

   docker ps  -a   # 查看执行状态

4. 如果执行出错，比如nginx启动出现端口冲突，导致nginx无法启动

   docker rm nginx-test  # 删除容器

   docker run -d -p  80:80 --name nginx-test daocloud.io/library/nginx  # 修改端口

   docker ps  -a   # 查看执行状态

   

   如果vagrant环境一致，那么现在可以通过 http://192.168.1.22/ 查看 nginx 的欢迎界面了！

   

   # 四、参考资源

   1.  vagrant box 查询下载网址

      https://app.vagrantup.com/boxes/search

   2. 文中使用的vagrant docker 镜像名称：williamyeh/centos7-docker 

   3. vagrantfile

      ````
      Vagrant.configure("2") do |config|
        config.vm.box = "williamyeh/centos7-docker"
        config.vm.network "public_network", ip: "192.168.1.22"  
      end
      ````

      

   

