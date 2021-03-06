# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
add-apt-repository ppa:duggan/bats --yes
apt-get update; apt-get install -y apache2 bats shellcheck curl
rm /var/www/html/index.html
tee /etc/apache2/sites-enabled/000-default.conf >/dev/null <<EOF
<VirtualHost *:80>
	ServerName example.org
	ServerAdmin tino@example.org
	DocumentRoot /var/www/html/
 
        ScriptAlias "/index.html" "/usr/lib/cgi-bin/index.cgi"
        ScriptAlias "/index" "/usr/lib/cgi-bin/index.cgi"
        RedirectMatch 404 index.htsh

        <Directory /var/www/html/>
          AllowOverride none
          Options -Indexes
          Require all granted
        </Directory>

	ErrorLog /var/log/apache2/error.log
	CustomLog /var/log/apache2/access.log combined

	Include conf-available/serve-cgi-bin.conf
</VirtualHost>
EOF
sed -i 's/www-data/vagrant/g' /etc/apache2/envvars
a2enmod cgid
service apache2 restart

cd /var/www/html && bats ./testing.bats
echo
echo "Got to http://localhost:8090"
SCRIPT

Vagrant.configure("2") do |config|
config.vm.define "bos" do |bos|
  bos.vm.hostname = "bos"
  bos.vm.box = "ubuntu/bionic64"
  bos.vm.provision "shell", inline: $script
  bos.vm.network "forwarded_port", guest: 80, host: 8090
  bos.vm.synced_folder ".", "/var/www/html"
  end
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
  end
end
