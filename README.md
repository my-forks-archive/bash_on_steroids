## Use !#bash scrpts for writing beautiful web apps  :sparkles:
![pageres](https://raw.githubusercontent.com/tinoschroeter/bash_on_steroids/master/static/like_a_boss.png)

### Features
- easy to use  ``` <?bash echo "hello world" ?>```
- write fast **wabapps** in **#!bash** script style
- **QUERY_STRING** and **POST_STRING** variables can used as normal bash **variables**
- (e.g example?var1=foo&var2=bar&var3=nase becomes to echo "${var1} ${var2} ${var3})

### Install Apache2
```shell
apt-get update; apt-get install -y apache2
tee /etc/apache2/sites-enabled/000-default.conf >/dev/null <<EOF
<VirtualHost *:80>
	ServerName example.org
	ServerAdmin webmaster@example.org
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
a2enmod cgid
service apache2 restart
```
### install BoS
```shell
mkdir -p /var/www/newProject
git clone https://github.com/tinoschroeter/bash_on_steroids.git /var/www/newProject
cd /var/www/newProject
./bos
```

### Usage:
All bash codes are to be enclosed within ``` <?bash ... ?> or in short, <? ... ?>``` tags. 
```html
<!DOCTYPE html>
<html>
 <body>
 <ul>
    <?bash
      for i in Buzz Rex Bo Hamm Slink Potato; do 
        echo "<li>$i</li>"
      done
      ?>
</ul>
</body>
</html>
```

## build
```shell
$ ./bos.sh 
$ index.htsh --->> /usr/lib/cgi-bin/index.cgi
```
### Vagrant
```shell
vagrant up
vagrant ssh
cd /var/www/html/
sudo ./bos.sh

open http://localhost:8080/
```


[Wiki -->](https://github.com/tinoschroeter/bash_on_steroids/wiki)
![wiki](https://raw.githubusercontent.com/tinoschroeter/bash_on_steroids/master/static/wiki.jpg "wiki")
