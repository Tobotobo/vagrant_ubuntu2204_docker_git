# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_version = "202401.31.0"

  config.vm.synced_folder ".", "/vagrant"

  # config.vm.network "private_network", type: "dhcp"
  config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network", type: "dhcp"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = "2"
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # タイムゾーンの設定
    apt-get install -y tzdata
    ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
    dpkg-reconfigure -f noninteractive tzdata

    # Docker のインストール
    apt-get update
    apt-get install -y ca-certificates curl
    install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    chmod a+r /etc/apt/keyrings/docker.asc
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null
    apt-get update
    apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    usermod -aG docker vagrant

    # Git のインストール
    apt-get install -y git

    # IP アドレス確認
    ip addr show eth1 | grep 'inet ' | awk '{print $2}' | cut -d/ -f1
  SHELL

end
