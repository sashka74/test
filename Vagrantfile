# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/8"
#  config.vm.box_check_update = false


  config.vm.define "haproxy" do |haproxy|
      haproxy.vm.network  "private_network", ip: "192.168.120.10"
      haproxy.vm.hostname = "haproxy"  
      haproxy.vm.network "forwarded_port", guest: 80, host: 8080
      haproxy.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
      haproxy.vm.provision "shell", inline: <<-SHELL
              mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
          SHELL
          haproxy.vm.provision "haproxy", type: "shell" do |s|
                s.inline = <<-SHELL            
                       sudo yum install haproxy -y
                       sudo mkdir -p /var/lib/haproxy
                       sudo touch /var/lib/haproxy/stats
                       sudo sed -i -r 's/server  app1 127.0.0.1:5001 check/server  web1 192.168.120.11:80 check cookie web1/' /etc/haproxy/haproxy.cfg
                       sudo sed -i -r 's/server  app2 127.0.0.1:5002 check/server  web2 192.168.120.12:80 check cookie web2/' /etc/haproxy/haproxy.cfg
                       sudo sed -i -r 's/server  app3 127.0.0.1:5003 check//' /etc/haproxy/haproxy.cfg
                       sudo sed -i -r 's/server  app4 127.0.0.1:5004 check//' /etc/haproxy/haproxy.cfg
                       sudo sed -i '70 a stats uri /haproxy?stats' /etc/haproxy/haproxy.cfg
                       sudo sed -i -r 's/stats uri /    stats uri /' /etc/haproxy/haproxy.cfg
                       sudo sed -i -r 's@:5000@:80@' /etc/haproxy/haproxy.cfg
                       sudo sed -i 81d /etc/haproxy/haproxy.cfg
                       sudo sed -i 80d /etc/haproxy/haproxy.cfg
                       sudo sed -i 79d /etc/haproxy/haproxy.cfg
                       sudo sed -i 73d /etc/haproxy/haproxy.cfg
                       sudo sed -i 72d /etc/haproxy/haproxy.cfg
                       sudo sed -i 70d /etc/haproxy/haproxy.cfg
                       sudo sed -i 69d /etc/haproxy/haproxy.cfg
                       sudo systemctl start haproxy
                       sudo systemctl enable haproxy
                SHELL
         end
      end
  end

  config.vm.define "web1" do |web1|
      web1.vm.network "private_network", ip: "192.168.120.11"
      web1.vm.hostname = "web1"  
      web1.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
      web1.vm.provision "shell", inline: <<-SHELL
              mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
          SHELL
          web1.vm.provision "web1", type: "shell" do |s|
                s.inline = <<-SHELL
                       setenforce 0
                       sudo yum install httpd wget unzip -y
                       sudo yum install php php-cli php-mysqlnd php-json php-gd php-ldap php-odbc php-pdo php-opcache php-pear php-xml php-xmlrpc php-mbstring php-snmp php-soap php-zip -y
                       wget https://files.phpmyadmin.net/phpMyAdmin/5.1.1/phpMyAdmin-5.1.1-all-languages.zip
                       unzip phpMyAdmin-5.1.1-all-languages.zip
                       sudo cp -R phpMyAdmin-5.1.1-all-languages/*  /var/www/html/
                       sudo cp /var/www/html/config.sample.inc.php /var/www/html/config.inc.php
                       echo -e '\$i++;\n\$cfg[\"Servers\"][\$i][\"host\"] = \"192.168.120.13\";\n\$cfg[\"Servers\"][\$i][\"port\"] = \"3306\";\n\$cfg[\"Servers\"][\$i][\"user\"] = \"myuser\";\n\$cfg[\"Servers\"][\$i][\"password\"] = \"mypass\";\n\$cfg[\"Servers\"][\$i][\"extension\"] = \"mysql\";\n\$cfg[\"Servers\"][\$i][\"auth_type\"] = \"config\";' | sudo tee -a /var/www/html/config.inc.php
                       sudo systemctl start httpd
                       sudo systemctl enable httpd
                SHELL
          end 
      end
 end
  config.vm.define "web2" do |web2|
      web2.vm.network "private_network", ip: "192.168.120.12"
      web2.vm.hostname = "web2"
      web2.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
      web2.vm.provision "shell", inline: <<-SHELL
              mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
          SHELL
          web2.vm.provision "web2", type: "shell" do |s|
                s.inline = <<-SHELL
                       setenforce 0
                       sudo yum install httpd wget unzip -y
                       sudo yum install php php-cli php-mysqlnd php-json php-gd php-ldap php-odbc php-pdo php-opcache php-pear php-xml php-xmlrpc php-mbstring php-snmp php-soap php-zip -y
                       wget https://files.phpmyadmin.net/phpMyAdmin/5.1.1/phpMyAdmin-5.1.1-all-languages.zip
                       unzip phpMyAdmin-5.1.1-all-languages.zip
                       sudo cp -R phpMyAdmin-5.1.1-all-languages/*  /var/www/html/
                       sudo cp /var/www/html/config.sample.inc.php /var/www/html/config.inc.php
                       echo -e '\$i++;\n\$cfg[\"Servers\"][\$i][\"host\"] = \"192.168.120.13\";\n\$cfg[\"Servers\"][\$i][\"port\"] = \"3306\";\n\$cfg[\"Servers\"][\$i][\"user\"] = \"myuser\";\n\$cfg[\"Servers\"][\$i][\"password\"] = \"mypass\";\n\$cfg[\"Servers\"][\$i][\"extension\"] = \"mysql\";\n\$cfg[\"Servers\"][\$i][\"auth_type\"] = \"config\";' | sudo tee -a /var/www/html/config.inc.php
                       sudo systemctl start httpd
                       sudo systemctl enable httpd
                SHELL
          end
      end
  end
 
  config.vm.define "mysql" do |mysql|
      mysql.vm.network "private_network", ip: "192.168.120.13"
      mysql.vm.hostname = "mysql"
      mysql.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
      mysql.vm.provision "shell", inline: <<-SHELL
              mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
          SHELL
          mysql.vm.provision "mysql", type: "shell" do |s|
                s.inline = <<-SHELL
                       sudo yum install mysql-server -y
                       sudo systemctl start mysqld
                       mysql -u root -e "CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypass'; GRANT ALL ON *.* TO 'myuser'@'localhost'; CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypass'; GRANT ALL ON *.* TO 'myuser'@'%'; FLUSH PRIVILEGES;"
                       sudo systemctl enable mysqld
                SHELL
          end
      end
  end


end
