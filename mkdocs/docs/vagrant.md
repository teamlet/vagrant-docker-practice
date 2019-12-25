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