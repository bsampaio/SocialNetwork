Vagrant.configure("2") do |config|
  config.vm.synced_folder "~/Projetos/SocialNetwork/source-api", "/var/source-api"
  config.vm.synced_folder "~/Projetos/SocialNetwork/source-client", "/var/source-client"
  
  config.vm.box = "ubuntu/trusty32"
  config.vm.network :private_network, ip: "192.168.56.2"
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "./cookbooks"
    
    chef.add_recipe "apt"
    chef.add_recipe "custom::dotdeb"
    chef.add_recipe "mysql::server"
    chef.add_recipe "nginx"
    chef.add_recipe "php"
    chef.add_recipe "php-fpm"
    chef.add_recipe "custom::fpm-config"
    chef.add_recipe "php::module_curl"
    chef.add_recipe "php::module_gd"
    chef.add_recipe "php::module_mysql"
    chef.add_recipe "postfix"
    chef.add_recipe "custom::db"
    chef.add_recipe "custom::hosts"
    chef.add_recipe "custom::vhosts"
    chef.add_recipe "custom::mysql_permissions"
    chef.add_recipe "custom::restart_services"
    
    chef.json = {
      :mysql => {
        :server_root_password => "root",
        :server_repl_password => "repl",
        :server_debian_password => "debian",
        :bind_address => '0.0.0.0'
      },
      :nginx => {
        :default_site_enabled => false
      },
      :php => {
        :directives => {
          'pdo_mysql.default_socket' => '/var/run/mysqld/mysqld.sock',
          'mysql.default_socket' => '/var/run/mysqld/mysqld.sock',
          'mysqli.default_socket' => '/var/run/mysqld/mysqld.sock',
          'date.timezone' => 'America/Sao_Paulo'
        }
      }
    }
  end
end
