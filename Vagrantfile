# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # 指定 box 以及修改主机名
  config.vm.box = "newiep/selfvagrant"
  config.vm.hostname = "newiep"

  # 端口映射，定义需要映射出来的端口
  default_ports = {
    # 80   => 8000,
    # 443  => 44300,
    # 3306 => 33060,
    # 5432 => 54320
  }

  default_ports.each do |guest, host|
    config.vm.network "forwarded_port", guest: guest, host: host, auto_correct: true
  end

  # 网络模式，下面是hostnoly模式，可以自己定义ip
  config.vm.network "private_network", ip: "192.168.33.11"

  # 也可以使用桥接模式，使用方法查看 vagrant 相关文档
  # config.vm.network "public_network"

  # 目录挂载，修改为自己需要挂在的目录
  config.vm.synced_folder "/Users/peiwen/Code", "/home/vagrant/Code"

  # 内存和 cpu 配置， vb.name 用来指定虚拟机名称
  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.name = "selfvagrant"
    vb.memory = "1024"
    vb.cpus   = 1
  end

end
