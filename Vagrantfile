# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.berkshelf.enabled = true

  config.vm.define :server do |server|
    server.omnibus.chef_version = :latest
    server.vm.hostname = "server"
    server.vm.box = "opscode-centos-6.5"
    server.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-6.5_chef-provisionerless.box"
    server.vm.network :private_network, ip: "192.168.33.10"
    server.vm.provider :virtualbox do |vb|
      # Don't boot with headless mode
      vb.gui = true
   
      # Use VBoxManage to customize the VM. For example to change memory:
      vb.customize ["modifyvm", :id, "--memory", "1024"]
    end

    server.vm.provision :chef_solo do |chef|
      chef.log_level = :debug
      chef.data_bags_path = "data_bags"
      chef.add_recipe "sensu-server-wrapper"
      chef.add_recipe "sensu-server-wrapper::check"
      chef.json = {
        :sensu => {
            :rabbitmq => {
              :host => "192.168.33.10",
            },
            :api => {
              :host => "192.168.33.10",
            },
            :graphite => {
              :host => "192.168.33.101",
              :port => 2003
            }
        }
      }
    end
  end

  config.vm.define :client01 do |client01|
    client01.omnibus.chef_version = :latest
    client01.vm.hostname = "client01"
    client01.vm.box = "opscode-centos-6.5"
    client01.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-6.5_chef-provisionerless.box"
    client01.vm.network :private_network, ip: "192.168.33.200"
    client01.vm.provider :virtualbox do |vb|
      # Don't boot with headless mode
      vb.gui = true
   
      # Use VBoxManage to customize the VM. For example to change memory:
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end

    client01.vm.provision :chef_solo do |chef|
      chef.log_level = :debug
      chef.data_bags_path = "data_bags"
      chef.add_recipe "sensu-client-wrapper"
      chef.add_recipe "dstat"
      chef.json = {
        "sensu-client-wrapper" => {
          :ipaddress => "192.168.33.200",
          :name => "client01",
          :roles => ["web"],
        },
        :sensu => {
            :rabbitmq => {
              :host => "192.168.33.10",
            },
        }
      }
    end
  end


  config.vm.define :client02 do |client02|
    client02.omnibus.chef_version = :latest
    client02.vm.hostname = "client02"
    client02.vm.box = "opscode-centos-6.5"
    client02.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-6.5_chef-provisionerless.box"
    client02.vm.network :private_network, ip: "192.168.33.201"
    client02.vm.provider :virtualbox do |vb|
      # Don't boot with headless mode
      vb.gui = true
   
      # Use VBoxManage to customize the VM. For example to change memory:
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end

    client02.vm.provision :chef_solo do |chef|
      chef.log_level = :debug
      chef.data_bags_path = "data_bags"
      chef.add_recipe "sensu-client-wrapper"
      chef.json = {
        "sensu-client-wrapper" => {
          :ipaddress => "192.168.33.201",
          :name => "client02",
          :roles => ["db"],
        },
        :sensu => {
            :rabbitmq => {
              :host => "192.168.33.10",
            },
        }
      }
    end
  end

  config.vm.define :graphite do |graphite|
    graphite.omnibus.chef_version = :latest
    graphite.vm.hostname = "graphite"
    graphite.vm.box = "opscode-ubuntu-12.04"
    graphite.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-12.04_chef-provisionerless.box"
    graphite.vm.network :private_network, ip: "192.168.33.101"
    graphite.vm.provider :virtualbox do |vb|
      # Don't boot with headless mode
      vb.gui = true
   
      # Use VBoxManage to customize the VM. For example to change memory:
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end

    graphite.vm.provision :chef_solo do |chef|
      chef.log_level = :debug
      chef.data_bags_path = "data_bags"
      chef.add_recipe "sensu-client-wrapper"
      chef.add_recipe "graphite"
      chef.json = {
        "sensu-client-wrapper" => {
          :ipaddress => "192.168.33.101",
          :name => "graphite",
          :roles => ["graphite"],
        },
        :sensu => {
            :rabbitmq => {
              :host => "192.168.33.10",
            },
        },
        :graphite => {
            :timezone => "Asia/Tokyo",
            :password => 'password',
            :apache => { 
                :basic_auth => {
                    :enabled => false
                }
            }
        }
      }
    end
  end

end
