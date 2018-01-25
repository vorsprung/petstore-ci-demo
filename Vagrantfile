#!/usr/bin/env ruby
# Creates an Ubuntu 16.04 VM
#   * Run using 'vagrant up'
#   * SSH using vagrant@192.168.10.10, password 'vagrant'

$script = <<SCRIPT
SOURCE=/vagrant/grails-petclinic
if [ "$SOURCE" == "" ]; then
    echo "SOURCE NOT SET ABORTING"
    exit 1
fi
# add a "no passwords" feature to sshd, just once
grep 'PasswordAuthentication' /etc/ssh/sshd_config
if [ "$?" != "0" ]; then
    echo "PasswordAuthentication no" >> /etc/ssh/sshd_config
fi
# look for systems updates, security updates etc
apt-get update
# install or upgrade required packages
apt-get -y install tomcat7 openjdk-8-jre nginx
# set up nginx config
rm -f /etc/nginx/sites-enabled/default
cp /vagrant/nginx-default /etc/nginx/sites-enabled/default

# copy WAR file into tomcat
systemctl stop tomcat7
chown tomcat7 /var/lib/tomcat7
rm -rf /var/lib/tomcat7/webapps/*
echo "copying in new WAR file and starting tomcat"
cp $SOURCE/target/*war /var/lib/tomcat7/webapps/ROOT.war
systemctl start tomcat7
systemctl restart nginx
SCRIPT
Vagrant.configure("2") do |config|
# set high timeout to prevent problem
  config.vm.boot_timeout = 900
  config.vm.box = "bento/ubuntu-16.04"
  config.vm.network "private_network", ip: "192.168.10.10"

    config.vm.provider "virtualbox" do |vb|
   	 vb.memory = "1024"
   	 vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
    end
  config.vm.provision "shell", inline: $script
end


