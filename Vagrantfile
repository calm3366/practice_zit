servers = [
  { hostname: "k8s-master", ip: "192.168.10.10" },
  { hostname: "k8s-worker1", ip: "192.168.10.11" },
  { hostname: "k8s-worker2", ip: "192.168.10.12" }
  # { hostname: "k8s-ingress", ip: "192.168.10.13" }
]

# Читаем ключ в Ruby
ssh_pub_path = File.expand_path("~/.ssh/id_rsa.pub")

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  # Указываем версию и отключаем проверку обновлений
  config.vm.box_version = ">= 1.0.0"
  config.vm.box_check_update = false
  # Если тебе не нужны shared folders, ты можешь просто отключить их
  config.vm.synced_folder ".", "/vagrant", disabled: true
  # Автоматическая установка Guest Additions с плагином vbguest
  config.vbguest.auto_update = true
  config.vbguest.no_remote = false
  # Настройки машины
  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.hostname = machine[:hostname]
      node.vm.network "private_network", ip: machine[:ip]
      node.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end
      # 👇 Передаём файл публичного ключа внутрь VM
      node.vm.provision "file", source: ssh_pub_path, destination: "/tmp/id_rsa.pub"
      # 👇 Добавляем SSH ключ через provisioning
      node.vm.provision "shell", inline: <<-SHELL
        mkdir -p /home/vagrant/.ssh
        cat /tmp/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
        chmod 700 /home/vagrant/.ssh
        chmod 600 /home/vagrant/.ssh/authorized_keys
        chown -R vagrant:vagrant /home/vagrant/.ssh
      SHELL
    end
  end
end