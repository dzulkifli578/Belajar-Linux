# Instruksi Instalasi LAMP di Arch Linux

## Update Repository dan Package
```bash
sudo pacman -Syu
sudo pacman -Syy

## Install LAMP Stack
sudo pacman -S apache php php-apache mysql phpmyadmin

## Start & Enable HTTP Daemon (Apache)
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

## Start & Enable MySQL Daemon
sudo systemctl start mysqld
sudo systemctl enable mysqld
sudo systemctl status mysqld

## Atasi Error MySQL (jika ada)
sudo su
cd /var/lib/mysql
rm -r *
mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

## Securing MySQL Installation
sudo mysql_secure_installation

## Login ke MySQL
mysql -u root -p

## Setting Apache
sudo nano /etc/httpd/conf/httpd.conf
### Pastikan LoadModule mpm_prefork & LoadModule athn-file_module ter-comment
LoadModule php_module modules/libphp.so
AddHandler php-script php
Include conf/extra/php_module.conf

## Restart & Cek Status HTTP Daemon
sudo systemctl restart httpd
sudo systemctl status httpd

## Tes Apache dan PHP
### Tes Apache
sudo nano /srv/http/index.html
## Isi kode HTML
<!DOCTYPE html>
<html>

<head>
  <title>Selamat Datang</title>
</head>

<body>
  <h1>Selamat Datang</h1>
  <p>Apache sudah diaktifkan</p>
</body>

</html>

### Tes PHP
sudo nano /srv/http/version.php
### Isi kode PHP
<?php
  phpinfo ()
?>

## Setting php.ini
sudo nano /etc/php/php.ini
### Uncomment extension=mysqli, extension=pdo_mysql, extension=iconv

## Setting PHPMyAdmin
bash
Copy code
sudo nano /etc/httpd/conf/extra/phpmyadmin.conf
### Isi kode file
Alias /phpmyadmin "/usr/share/webapps/phpMyAdmin"
<Directory "/usr/share/webapps/phpMyAdmin">
	DirectoryIndex  index.php
    AllowOverride All
    Options FollowSymlinks
    Require all granted
</Directory>

### Masukkan path konfigurasi phpmyadmin ke httpd.conf
sudo nano /etc/httpd/conf/httpd.conf
### Di akhir file tambahkan baris ini
Include conf/extra/phpmyadmin.conf

## Restart Apache
sudo systemctl restart httpd

## Tambahkan User untuk Login PHPMyAdmin
sudo mysql -u root -p
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
FLUSH PRIVILEGES;

## Referensi
https://www.ubuntumint.com/install-lamp-archlinux/
https://stackoverflow.com/questions/36864206/sqlstatehy000-1698-access-denied-for-user-rootlocalhost
