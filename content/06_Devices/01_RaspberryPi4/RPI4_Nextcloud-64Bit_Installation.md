#Devices 

---
apt install docker
apt install docker-compose






install some packages: see: https://jet0jlh.de/?p=380
system requirements, see: https://docs.nextcloud.com/server/latest/admin_manual/installation/system_requirements.html

plug in storage medium

#### automatically mount storage-medium:
lsblk
fdisk /dev/sda
g
n
w
mkfs.ext4 /dev/sda1
mkdir /media/ssd

copy uuid:
blkid
nano /etc/fstab
/dev/disk/by-uuid/UUID        /media/ssd       ext4         nofail          0         2

check:
mount -a
lsblk


#### <mark style="background: #D2B3FFA6;">!(configure RAID system)</mark>


#### installation of webserver
get certificate (rsa-key):
mkdir /var/cert
openssl genrsa -out server.key
openssl req -new -key server.key -out server.csr -sha256
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

mkdir /var/www/nextcloud
mkdir /media/SSD1/nextcloud
chown www-data:www-data /media/SSD1/nextcloud/
chown www-data:www-data /var/www/nextcloud/

nano /etc/apache2/apache2.conf
<Directory /media/SSD1/nextcloud>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>

virtual host:
nano /etc/apache2/sites-available/000-default.conf
DocumentRoot /var/www/nextcloud
```cmd
<IfModule mod_ssl.c>
	<VirtualHost *:443>
		DocumentRoot /var/www/nextcloud
		SSLEngine on
		SSLCertificateFile /var/cert/server.crt
		SSLCertificateChainFile /var/cert/server.csr
		SSLCertificateKeyFile /var/cert/server.key
	</VirtualHost>
</IfModule>
```

auto-complete to https:
```cmd
<IfModule mod_rewrite.c>
	RewriteEngine On
	RewriteCond %{HTTPS} off
	RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```
systemctl restart apache2


#### configure mariadb
mariadb -u root -p
"password"
create user cloud@localhost identified by "root1234/";
create database cloud;
show databases;
grant all privileges on cloud.* to cloud@localhost;

check:
mariadb -u cloud -p
"password"
use cloud;


#### nextcloud installation ~ez
cd /var/www
wget https://download.nextcloud.com/server/releases/latest.zip
unzip latest.zip
chown -R www-data:www-data nextcloud

browser:
https://raspberrypi.local/index.php
username
"password"
/media/SSD1/nextcloud
cloud
"password"
cloud
localhost
>Install


#### optimization
installation of open php-modules:
apt install php8.2-intl php8.2-imagick php8.2-gmp php8.2-bcmath
systemctl restart apache2
apt install libmagickcore-6.q16-6-extra
nano /etc/php/8.2/apache2/php.ini
memory_limit = 4096M
upload_max_filesize = 32G
max_file_uploads = 1000