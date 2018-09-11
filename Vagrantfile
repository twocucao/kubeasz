Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false
  config.vm.provider "virtualbox"

  $kube_master_node = [
    {
      "host"=> "kube-node-master", # kube master / etcd
      "ip" => "10.64.3.100",
    }
  ]

  $kube_master_node_pub_key = ""

  $kube_master_node.each do |node_info|
    config.vm.define node_info["host"] do |node|
      node.vm.hostname = node_info["host"]
      node.vm.network :private_network, ip: node_info["ip"],  auto_config: true
      node.vm.provider :virtualbox do |vb, override|
        vb.name = node_info["host"]
        vb.gui = false
        vb.memory = 2048
        vb.cpus = 1
      end
      config.vm.provision "shell" do |s|
          s.inline = <<-SHELL
          yes | ssh-keygen -b 2048 -t rsa -f /home/vagrant/.ssh/id_rsa -q -N ""
          yes | ssh-keygen -b 2048 -t rsa -f /root/.ssh/id_rsa -q -N ""
          SHELL
          ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
          $kube_master_node_pub_key = ssh_pub_key
          s.inline = <<-SHELL
          echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
          echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
          SHELL
      end
    end
  end

  $kube_info_nodes = [
    {
      "host"=> "kube-node-101",
      "ip" => "10.64.3.101",
    },
    {
      "host"=> "kube-node-102",
      "ip" => "10.64.3.102",
    },
  ]
  $kube_info_nodes.each do |node_info|
    config.vm.define node_info["host"] do |node|
      node.vm.hostname = node_info["host"]
      node.vm.network :private_network, ip: node_info["ip"],  auto_config: true
      node.vm.provider :virtualbox do |vb, override|
        vb.name = node_info["host"]
        vb.gui = false
        vb.memory = 2048
        vb.cpus = 1
      end
      config.vm.provision "shell" do |s|
          ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
          # 本地 authorized_keys
          s.inline = <<-SHELL
          echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
          echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
          echo #{$kube_master_node_pub_key} >> /home/vagrant/.ssh/authorized_keys
          echo #{$kube_master_node_pub_key} >> /root/.ssh/authorized_keys
          SHELL
      end
    end
  end
end
