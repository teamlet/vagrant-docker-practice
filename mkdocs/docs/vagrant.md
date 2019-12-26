# vagrant 技术


## vagrant  官网

*  https://www.vagrantup.com/

## vagrant 常用命令

* `vagrant box add ([box-name]|[url])`

  下载指定的镜像到本地。`box-name` 在创建虚拟环境的时候使用。
  
* `vagrant box list`

   查看本地下载完的 box 列表。
  
* `vagrant init [box-name]`

   初始化vagrantfile文件。在一个新的文件夹中，执行此命令，会创建一个 vagrantfile。
   
   如果没提供 `box-name` ，vargantfile 使用 `base` 作为box的名字，但是不能启动。只能作为模板使用。
  
   否则，vargantfile 使用 `box-name` 作为box的名字。
   
 * `vagrant up` 
 
    通过vagrantfile启动虚拟环境。这个命令会读取vagrantfile文件，创建一个 .vagrant 目录，保存当前虚拟环境的信息。
    在虚拟环境使用过程中，如果出现不可恢复的问题，可以直接删除 .vagrant 文件夹，执行 `vagrant up` 重建新的虚拟环境。

* `vagrant ssh`

    通过 `vagrant up`启动默认配置的虚拟环境后，没有使用 virtualBox 的交互界面。 除非手动修改配置，否则就需要通过 SSH 连接虚拟环境。
    执行此命令后，本主机直接进入虚拟环境。此后，在虚拟环境中，用户名和密码都是 `vagrant`。  


* 更新本地环境中指定的box

　　`vagrant box update box-name`

* 删除本地环境中指定的box

　　`vagrant box remove box-name`

* 重新打包本地环境中指定的box

　　`vagrant box repackage box-name`

* 在空文件夹初始化虚拟机

　　`vagrant init [box-name]`

* 在初始化完的文件夹内启动虚拟机

　　`vagrant up`

* ssh登录启动的虚拟机

　　`vagrant ssh`

* 挂起启动的虚拟机

　　`vagrant suspend`

* 重启虚拟机

　　`vagrant reload`

* 关闭虚拟机

　　`vagrant halt`

* 查找虚拟机的运行状态

　　`vagrant status`

* 销毁当前虚拟机

　　`vagrant destroy`

## vagrant 环境设置

 * vagrant 主目录设置
 
  vagrant 安装完成之后，默认的主目录是当前用户的文件夹 .vagrant.d目录 。主目录保存下载的所有镜像box，如果使用多个镜像box，系统磁盘空间恐怕不够。
 因此，需要更改vagrant的主目录。( Windows 当前用户文件夹指的是  C:\users\current-user-name )
 
  在windows下，可以通过设置 VAGRANT_HOME 来指定主目录，比如:文件夹 d:\vagrent-home。
  在windows 10 Home下，在非系统盘创建一个文件夹，指定为主目录。
    
  需要注意的是，不要使用已有的文件夹，而是在盘符下，比如：D:\盘下直接创建主目录，否则会有读写权限的问题。
    
 * virtualBox 全局设置
 
   virtualBox 有一个全局设定，其中有一个是创建的虚拟环境(虚拟电脑、虚拟主机)的保存位置。
   这个虚拟主机文件是vagrant 以镜像 box为模板，通过vagrantfile配置，创建的可运行的实例。
   这个位置默认是当前用户的VirtualBox VMs目录。
   
   这个设定可以通过
   
   * 修改当前用户文件夹 .VirtualBox 目录下的 VirtualBox.xml ，defaultMachineFolder 完成；
   * 或者打开 virtualBox程序，通过全局设定界面修改。
   
   这样，执行 `vagrant up` 创建的虚拟主机就会保存到这个全局设定位置。