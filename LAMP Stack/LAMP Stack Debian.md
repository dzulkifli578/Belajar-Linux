# instal LAMP Stack Debian

## update repository & upgrade package
```bash
sudo apt update && sudo apt upgrade -y
```

## install package yang diperlukan LAMP
```bash
sudo apt install apache2* ufw -y php libapache2-mod-php php-mysql -y
```

## cek versi package masing-masing
```bash
sudo apache2 -v
mariadb -v
php -v
```

## cek info PHP
```bash
sudo nano /var/www/html/version.php
```
### tulis kode PHP
```bash
<?php
    phpinfo ();
?>
```

## Aktivasi Apache
```bash
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

## Aktivasi UFW
```bash
sudo ufw enable
sudo ufw allow http
sudo ufw status
```

## Aktivasi MySQL
```bash
sudo systemctl start mariadb 
sudo systemctl enable mariadb 
sudo systemctl status mariadb 
```

## mengamankan instalasi MySQL
```bash
sudo mariadb-secure-installation
```

## Login ke MySQL
```bash
sudo mariadb -u root -p
```

## Tambahkan User untuk Login PHPMyAdmin
```bash
sudo mysql -u root -p
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
```

## Referensi
https://www.ubuntumint.com/install-lamp-apache-mariadb-php-on-debian-12/
https://stackoverflow.com/questions/36864206/sqlstatehy000-1698-access-denied-for-user-rootlocalhost