# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

VAGRANTFILE_API_VERSION ||= "2"
confDir = $confDir ||= File.expand_path("./config")

configYamlPath = confDir + "/config.yaml"
afterScriptPath = confDir + "/after.sh"
aliasesPath = confDir + "/aliases"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Prevent TTY Errors
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  # Allow SSH Agent Forward from The Box
  config.ssh.forward_agent = true
  
  if File.exist? aliasesPath then
      config.vm.provision "file", source: aliasesPath, destination: "~/.bash_aliases"
  end

  if File.exist? configYamlPath then
      settings = YAML::load(File.read(configYamlPath))
  end

  # Configure The Box
  config.vm.define settings["name"] ||= "workstation"
  config.vm.box = settings["box"] ||= "newiep/workstation"
  config.vm.box_version = settings["version"] ||= ">= 0.0.1"
  config.vm.hostname = settings["hostname"] ||= "newiep"

  # 网络模式，下面是hostnoly模式，可以自己定义ip
  config.vm.network "private_network", ip: settings["ip"] ||= "192.168.33.11"

  # 内存和 cpu 配置， vb.name 用来指定虚拟机名称
  config.vm.provider "virtualbox" do |vb|
    vb.name   = settings["name"] ||= "workstation"
    vb.memory = settings["memory"] ||= "1024"
    vb.cpus   = settings["cpu"] ||= 1
  end

  # 端口映射
  if settings.has_key?("ports")
    settings["ports"].each do |port|
      config.vm.network "forwarded_port", guest: port["guest"], host: port["host"], protocol: port["protocol"], auto_correct: true
    end
  end

  # 为 ssh 链接配置公钥
  if settings.include? 'authorize'
    if File.exists? File.expand_path(settings["authorize"])
      config.vm.provision "shell" do |s|
        s.inline = "echo $1 | grep -xq \"$1\" /home/vagrant/.ssh/authorized_keys || echo \"\n$1\" | tee -a /home/vagrant/.ssh/authorized_keys"
        s.args = [File.read(File.expand_path(settings["authorize"]))]
      end
    end
  end

  # 配置共享目录
  if settings.include? 'folders'
    settings["folders"].each do |folder|
      config.vm.synced_folder folder["map"], folder["to"], type: folder["type"] ||= nil
    end
  end
  
  # 更新 composer 每次 --provision
  config.vm.provision "shell" do |s|
    s.inline = "/usr/local/bin/composer self-update"
  end

  if File.exist? afterScriptPath then
      config.vm.provision "shell", path: afterScriptPath, privileged: false
  end

end
