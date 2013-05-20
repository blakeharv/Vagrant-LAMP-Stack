# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  # Define VM box to use
  config.vm.box = "MyPreciseBox"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Increases memory allocated for VM.
  #config.vm.customize ["modifyvm", :id, "--memory", 4096, "--cpus", 2]

  # Define hostname to be used with Hostmaster
  config.vm.host_name = "server.dev"
  #config.hosts.name = "server.dev"

  # Use hostonly network with a static IP Address
  config.vm.network :hostonly, "172.90.90.90"

  # Set share folder
  config.vm.share_folder('www', '/home/vagrant/www', '../www', create: true, nfs: true)

  # Enable and configure chef solo
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe "apt"
    chef.add_recipe "postfix"
    chef.add_recipe "openssl"
    chef.add_recipe "apache2"
    chef.add_recipe "apache2::mod_php5"
    chef.add_recipe "apache2::mod_rewrite"
    chef.add_recipe "apache2::mod_ssl"
    chef.add_recipe "mysql"
    chef.add_recipe "mysql::server"
    chef.add_recipe "memcached"
    chef.add_recipe "misc::packages"
    chef.add_recipe "misc::vhost"
    chef.add_recipe "misc::db"
    chef.add_recipe "php"
    chef.add_recipe "phpmyadmin"
    chef.json = {
      :misc => {
        # Project name
        :name           => "server",

        # Name of MySQL database that should be created
        :db_name        => "dbname",

        # Optional database dump to be imported when server is provisioned
        # If the file doesn't exist, it is just ignored
        :db_dump        => "/home/vagrant/shared/dump.sql",

        # Server name and alias(es) for Apache vhost
        :server_name    => "server.dev",
        :server_aliases => "*.server.dev",

        # Document root for Apache vhost
        :docroot        => "/home/vagrant/www",
      },
      :mysql => {
        :server_root_password   => 'root',
        :server_repl_password   => 'root',
        :server_debian_password => 'root',
        :bind_address           => '172.90.90.90',
        :allow_remote_root      => true
      },
      :phpmyadmin => {
        :cfg => {
          :control_user_password => 'password'
        },
        :webserver => 'apache2'
      }
    }
  end
end