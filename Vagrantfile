# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
        :box_name => "ubuntu/22.04",
        :ansible => 'inetRouter.yml',
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                   {ip: '192.168.56.10', adapter: 3, netmask: "255.255.255.0"},
                ]
  },
  :centralRouter => {
        :box_name => "ubuntu/22.04",
        :ansible => 'centralRouter.yml',
        :net => [
                   {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                   {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {ip: '192.168.0.33', adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "hw-net"},
                   {ip: '192.168.0.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "wifi-net"},
                   {ip: '192.168.255.9', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"},
                   {ip: '192.168.255.5', adapter: 7, netmask: "255.255.255.252", virtualbox__intnet: "office2-central"},
                   {ip: '192.168.56.11', adapter: 8, netmask: "255.255.255.0"},
                ]
  },
  
  :centralServer => {
        :box_name => "ubuntu/22.04",
        :ansible => 'centralServer.yml',
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {ip: '192.168.56.12', adapter: 3, netmask: "255.255.255.0"},
                ]
  },
  :office1Router => {
        :box_name => "ubuntu/22.04",
        :ansible => 'office1Router.yml',
        :net => [
                   {ip: '192.168.255.10', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"},
                   {ip: '192.168.2.65', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "test1-net"},
                   {ip: '192.168.2.1', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "dev1-net"},
                   {ip: '192.168.2.129', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "mnt-net"},
                   {ip: '192.168.2.193', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "off1-net"},
                   {ip: '192.168.56.13', adapter: 7, netmask: "255.255.255.0"},
            ]
  },
  :office2Router => {
        :box_name => "ubuntu/22.04",
        :ansible => 'office2Router.yml',
        :net => [
                   {ip: '192.168.255.6', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office2-central"},
                   {ip: '192.168.1.129', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "test2-net"},
                   {ip: '192.168.1.1', adapter: 4, netmask: "255.255.255.128", virtualbox__intnet: "dev2-net"},
                   {ip: '192.168.1.193', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "off2-net"},
                   {ip: '192.168.56.14', adapter: 6, netmask: "255.255.255.0"},
            ]
  },
  :office1Server => {
        :box_name => "ubuntu/22.04",
        :ansible => 'office1Server.yml',
        :net => [
                   {ip: '192.168.2.130', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "mnt-net"},
                   {ip: '192.168.56.15', adapter: 3, netmask: "255.255.255.0"},
            ]
  },
  :office2Server => {
        :box_name => "ubuntu/22.04",
        :ansible => 'office2Server.yml',
        :net => [
                   {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "dev2-net"},
                   {ip: '192.168.56.16', adapter: 3, netmask: "255.255.255.0"},
            ]
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", **ipconf
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        if boxconfig.key?(:ansible) 
          box.vm.provision "ansible" do |ansible|
            ansible.playbook = boxconfig[:ansible]
          end
        end

      end

  end
  
  
end
