**Instruksi Instalasi WordPress + LAMP Stack**

1. [Install LAMP Stack](#1-install-lamp-stack)
- [a. Mempersiapkan Dependency LAMP Stack](#a-mempersiapkan-dependency-lamp-stack)
  - [Update Repository & Upgrade Package](#update-repository--upgrade-package)
  - [Install Package yang Diperlukan LAMP](#install-package-yang-diperlukan-lamp)
  - [Cek Versi Package Masing-Masing](#cek-versi-package-masing-masing)
- [b. Mengetes PHP](#b-mengetes-php)
  - [Cek Info PHP](#cek-info-php)
  - [Tulis Kode PHP](#tulis-kode-php)
- [c. Aktivasi Layanan Dependency LAMP Stack](#c-aktivasi-layanan-dependency-lamp-stack)
  - [Aktivasi Apache](#aktivasi-apache)
  - [Aktivasi UFW](#aktivasi-ufw)
  - [Aktivasi MySQL](#aktivasi-mysql)
- [d. Mengatur MySQL](#d-mengatur-mysql)
  - [Mengamankan Instalasi MySQL](#mengamankan-instalasi-mysql)
  - [Login ke MySQL](#login-ke-mysql)
  - [Tambahkan User untuk Login PHPMyAdmin](#tambahkan-user-untuk-login-phpmyadmin)
2. [Install WordPress](#2-install-wordpress)
- [a. Mempersiapkan WordPress](#a-mempersiapkan-wordpress)
  - [Download WordPress dan Ekstrak ke Directory `wordpress_user`](#download-wordpress-dan-ekstrak-ke-directory-wordpress_ser)
  - [Berikan Akses Apache ke Directory `wordpress_user`](#berikan-akses-apache-ke-directory-wordpress_user)
- [b. Konfigurasi MySQL untuk WordPress](#b-konfigurasi-mysql-untuk-wordpress)
  - [Buat Database untuk WordPress](#buat-database-untuk-wordpress)
  - [Mengatur Konfigurasi Database WordPress](#mengatur-konfigurasi-database-wordpress)
  - [Atur Potongan Kode pada `wp-config.php`](#atur-potongan-kode-pada-wp-configphp)
3. [Referensi](#3-referensi)

## 1. Install LAMP Stack
### a. Mempersiapkan Dependency LAMP Stack
#### Update Repository & Upgrade Package
```bash
sudo apt update && sudo apt upgrade -y
```
#### Install Package yang Diperlukan LAMP
```bash
sudo apt install apache2* ufw -y php libapache2-mod-php php-mysql -y
```
#### Cek Versi Package Masing-Masing
```bash
sudo apache2 -v
mariadb -v
php -v
```
### b. Mengetes PHP
#### Cek Info PHP
```bash
sudo nano /var/www/html/version.php
```
##### Tulis Kode PHP
```bash
<?php
    phpinfo ();
?>
```
### c. Aktivasi Layanan Dependency LAMP Stack
#### Aktivasi UFW Apache MySQL
```bash
sudo ufw enable
sudo ufw allow http
sudo ufw status
sudo systemctl start apache2 mariadb
sudo systemctl enable apache2 mariadb
sudo systemctl status apache2 mariadb
```
### d. Mengatur MySQL
#### Mengamankan Instalasi MySQL
```bash
sudo mariadb-secure-installation
```
#### Tambahkan User untuk Login PHPMyAdmin
```bash
sudo mariadb
CREATE USER 'root'@'localhost' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'root';
FLUSH PRIVILEGES;
QUIT;
```

## 2. Install WordPress
### a. Mempersiapkan WordPress
#### Download WordPress dan Ekstrak ke Directory `wordpress_user`
```bash
sudo mkdir /var/www/wordpress_user
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvf ./latest.tar.gz -C /var/www/wordpress_user
```
#### Berikan Akses Apache ke Directory `wordpress_user`
```bash
sudo chown -R www-data:www-data /var/www/wordpress_user
sudo chmod -R 755 /var/www/wordpress_user
```
### b. Konfigurasi MySQL untuk WordPress
#### Buat Database untuk WordPress
```bash
sudo mariadb
CREATE DATABASE wordpress;
FLUSH PRIVILEGES;
QUIT;
```
#### Mengatur Konfigurasi Database WordPress
```bash
cd /var/www/wordpress_user/wordpress
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```
#### Atur Potongan Kode pada `wp-config.php`
```bash
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wpUser' );
define( 'DB_PASSWORD', 'wpPass' );
```

## 3. Referensi
* https://www.ubuntumint.com/install-lamp-apache-mariadb-php-on-debian-12/
* https://stackoverflow.com/questions/36864206/sqlstatehy000-1698-access-denied-for-user-rootlocalhost
* https://www.ubuntumint.com/install-wordpress-on-ubuntu/
