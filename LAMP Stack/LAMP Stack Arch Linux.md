# Instruksi Instalasi LAMP di Arch Linux

## Update Repository dan Package
```bash
sudo pacman -Syu
sudo pacman -Syy
```

## Install LAMP Stack
```bash
sudo pacman -S apache php php-apache mysql phpmyadmin
```
## Start & Enable HTTP Daemon (Apache)
```bash
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

## Start & Enable MySQL Daemon
```bash
sudo systemctl start mysqld
sudo systemctl enable mysqld
sudo systemctl status mysqld
```

## Atasi Error MySQL (jika ada)
```bash
sudo su
cd /var/lib/mysql
rm -r *
mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

## Mengamankan Instalasi MySQL
```bash
sudo mysql_secure_installation
```

## Login ke MySQL
```bash
mysql -u root -p
```

## Setting Apache
```bash
sudo nano /etc/httpd/conf/httpd.conf
```
### Pastikan LoadModule mpm_prefork & LoadModule athn-file_module ter-comment
```bash
LoadModule php_module modules/libphp.so
AddHandler php-script php
Include conf/extra/php_module.conf
```

## Restart & Cek Status HTTP Daemon
```bash
sudo systemctl restart httpd
sudo systemctl status httpd
```

## Tes Apache dan PHP
### Tes Apache
```bash
sudo nano /srv/http/index.html
```
## Isi kode HTML
```bash
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
```
### Tes PHP
```bash
sudo nano /srv/http/version.php
```
### Isi kode PHP
```bash
<?php
  phpinfo ()
?>
```

## Setting php.ini
```bash
sudo nano /etc/php/php.ini
```
### Uncomment extension=mysqli, extension=pdo_mysql, extension=iconv

## Setting PHPMyAdmin
```bash
sudo nano /etc/httpd/conf/extra/phpmyadmin.conf
```
### Isi kode file
```bash
Alias /phpmyadmin "/usr/share/webapps/phpMyAdmin"
<Directory "/usr/share/webapps/phpMyAdmin">
	DirectoryIndex  index.php
    AllowOverride All
    Options FollowSymlinks
    Require all granted
</Directory>
```
### Masukkan path konfigurasi phpmyadmin ke httpd.conf
```bash
sudo nano /etc/httpd/conf/httpd.conf
```
### Di akhir file tambahkan baris ini
```bash
Include conf/extra/phpmyadmin.conf
```

## Restart Apache
```bash
sudo systemctl restart httpd
```

## Tambahkan User untuk Login PHPMyAdmin
```bash
sudo mysql -u root -p
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
```

## Referensi
```bash
https://www.ubuntumint.com/install-lamp-archlinux/
https://stackoverflow.com/questions/36864206/sqlstatehy000-1698-access-denied-for-user-rootlocalhost
```
