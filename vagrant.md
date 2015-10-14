# 使用Vagrant搭建Hadoop学习环境

学习Hadoop最烦人的地方就是刚开始环境的搭建，
要搭建一个Hadoop集群，可能很多人就卡在这了。
其实并不是Hadoop集群难于搭建，而是机器难求，
土豪可能有自己的云环境，虚拟机随便申请，屌丝
就只能在个人电脑上装多个虚拟机，很是麻烦。
还好遇到了vagrant，让虚拟机管理起来更加简单了。

> Note: 本书的所有例子都是在ubuntu上进行的。

## 安装Vagrant

Vagrant支持virtualbox和vmware，由于vmware是收费的，
所以这里使用virtualbox。


```
sudo apt-get install virtualbox
sudo apt-get install vagrant
```

## 下载官方的ubuntu镜像

Ubuntu precise 64 VirtualBox <http://files.vagrantup.com/precise64.box>

```
$ mkdir ~/box
$ cd ~/box
$ wget http://files.vagrantup.com/precise64.box
```

## 将镜像添加到Vagrant

```
$ vagrant box add ubuntu ~/box/precise64.box
```

## 使用镜像初始化实例

```
$ mkdir ~/vm/single_hadoop
$ cd ~/vm/single_hadoop
$ vagrant init ubuntu
```

## 修改网络配置

初始化后，会生成Vagrantfile文件，此文件是虚拟机启动相关配置。
我们需要调整下网络配置，将下面一句的注释#去掉：

```
#config.vm.network : public_network
```

这样相当于桥接的方式联网。当然如果你需要别的方式，可以自己选择。

## 启动虚拟机

```
$ cd ~/vm/single_hadoop
$ vagrant up
```

## 登录虚拟机

```
$ cd ~/vm/single_hadoop
$ vagrant ssh
vagrant@precise64:~$
```
进去之后，直接是vagrant用户，密码也是vagrant。

## 安装JAVA

```
$ sudo apt-get update
$ sudo apt-get install openjdk-7-jre
$ sudo apt-get install openjdk-7-jdk
```

设置JAVA_HOME，在~/.bashrc 加入如下：

```
export JAVA\_HOME=/usr/lib/jvm/java-7-openjdk-amd64/
```

## 下载Hadoop可执行包

```
$ wget http://mirror.bit.edu.cn/apache/hadoop/common/stable/hadoop-2.7.1.tar.gz
$ tar -xzvf hadoop-2.7.1.tar.gz
$ ln -s hadoop-2.7.1 hadoop
```

## 测试单机模式

```
$ cp etc/hadoop/*.xml input
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar grep input output 'dfs[a-z.]+'
$ cat output/*
```

## 下载hadoop-book

hadoop-book是本书作者在github上的代码库，对应书中的代码。

```
$ git clone https://github.com/tomwhite/hadoop-book.git
```

## 编译hadoop-book

```
$ mvn package -DskipTests
```

> Note: hadoop-book需要maven3的版本，需要手动安装



