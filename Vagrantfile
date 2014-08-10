# encoding: utf-8

Vagrant.configure("2") do |config|

  config.vm.box = "opscode-ubuntu-12.04_chef-11.4.0"
  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/opscode_ubuntu-12.04_chef-11.4.0.box"
  
  config.vm.synced_folder "src", "/var/www/mpr/src", create:true
  config.vm.synced_folder "media", "/home/sysx/myphysio/media", create:true
  config.vm.synced_folder "scripts", "/var/www/scripts", create:true
  config.vm.synced_folder "data", "/var/www/data", create:true

 
  config.vm.network "private_network", ip: "22.22.22.22"
  config.vm.network :forwarded_port, guest: 5432, host: 5433
  
  config.ssh.forward_agent = true

  #prevent stdin is not a tty error in vagrant
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe "apt"
    chef.add_recipe "nodejs"
    chef.add_recipe "openssl"
    chef.add_recipe "build-essential"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "redis2"
    chef.add_recipe "nginx"
    chef.add_recipe "screen"
    chef.json = {
      "nodejs"=>{
        "version"=>"0.10.26"
      },
      "postgresql"=>{
        "password"=>{
          "postgres"=>"postgres"
        }
      }
    }
    
  end

#install npm modules
config.vm.provision :shell, :inline => "cd /var/www/mpr/src/; sudo npm install;"

#configure nginx virtual host
config.vm.provision :shell, :inline => "sudo cp /var/www/scripts/nginx-mpr-vhost.conf /etc/nginx/sites-available/mpr"
config.vm.provision :shell, :inline => "sudo ln -s /etc/nginx/sites-available/mpr /etc/nginx/sites-enabled/mpr"
config.vm.provision :shell, :inline => "sudo service nginx restart"

#start redis
config.vm.provision :shell, :inline => "sudo service redis-server start"


#set password for postgres user
config.vm.provision :shell, :inline => "sudo echo -e 'postgres\npostgres' | sudo passwd postgres"
#create postgresql database
config.vm.provision :shell, :inline => "sudo -u postgres psql -c 'create database physiorehab'"
#restore database data from backup
config.vm.provision :shell, :inline => "sudo -u postgres psql physiorehab < /var/www/data/physiorehab.backup"
  
end