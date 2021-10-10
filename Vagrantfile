$script_mysql = <<-SCRIPT
  apt-get update && \
  apt-get install -y mysql-server-5.7 && \
  mysql < /vagrant/mysql/user.sql && \
  cat /vagrant/mysql/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf && \
  service mysql restart
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.network "forwarded_port", guest: 80, host: 80
  config.vm.network "forwarded_port", guest: 3306, host: 3306
 
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
  SHELL

  config.vm.define "mysqlserver" do |mysqlserver|
    mysqlserver.vm.provider "virtualbox" do |vb|
      mysqlserver.vm.network "private_network", ip: "192.168.33.10"
      vb.name = "mysqlserver"
    end
  
    mysqlserver.vm.provision "shell", inline: $script_mysql
  end

end
