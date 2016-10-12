# selfvagrant
使用 vagrant 搭建 php 开发环境

# 从无到有

## 准备工作

###  安装 vagrant

下载 [vagrant 官网地址](https://www.vagrantup.com/downloads.html) 并安装

### 安装 virtual box
下载 [VirtualBox 官网地址](https://www.virtualbox.org/wiki/Downloads) 并安装

### 克隆仓库到本地
```
  git clone git@github.com:newiep/selfvagrant.git vagrant
  cd vagrant
```

## 开始搭建

### 添加 box 到 vagrant
```
  vagrant box add metadata.json
```
> 我已经把准备好的环境打包并上传到七牛，上面的命令会从七牛下载 box 文件并添加到 vagrant， 中间可能会等待一些时间

### 查看是否已经添加成功

```
  vagrant box list
```
如果看到  newiep/workstation (virtualbox, 0.0.1) 的列表，证明添加成功

### 启动环境

```
vagrant up
```

耐心等环境启动完毕就可以了，然后可以用 `vagrant ssh` 进入到虚拟机里，happy coding ^_^
