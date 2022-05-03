Установил VirtualBox  и в нее установил ubuntu


	Установка nginx
Обновился - 
sudo apt update

установил вебсервер - 
sudo apt install nginx

посмотреть статус сервера - 
service nginx status

добавим программу в автозагрузку - 
sudo systemctl enable nginx

главный файл конфигурации - 
sudo vi /etc/nginx/nginx.conf

проверяем на ошибки - 
nginx -t

перезагружаем сервер - 
sudo service nginx reload
sudo service nginx restart

Посмотрел главный файл конфигурации - настройки по умолчанию подходят.


	Установка PHP
просмотр информации (8.1+92ubuntu1) - sudo apt show php

для установки версии из репозиториев - sudo apt install php

установил основные расширения - 
sudo apt install php-curl php-memcached php-mysql php-pgsql php-gd 
php-imagick php-intl php-xml php-zip php-mbstring


	УСТАНОВКА PHP-FPM В UBUNTU
установка php-fpm и модулей:
sudo apt -y install php-cli php-fpm php-json php-pdo php-pear php-bcmath

проверяем статус PHP-FPM:
systemctl status php8.1-fpm.service


	Настройка nginx and php-fpm

cd /var/www/			- заходим дирректорию www

sudo chown -R $USER:$USER html	- назначам текущего юзера владельцем папки (drwxr-xr-x  2 valery valery 4096 апр 29 20:31 html/)

cd html				- заходим в папку html

mkdir test			- создаю новую дирректорию в папке html

cd test				- заходим

nano index.php			- входим в nano создаем файл index.php

echo 'Hello World!'	- добавляем запись в файл

добавляем тестовую папку в список доступных доменов сервера - 
cd /etc/nginx/sites-available/ 

скопируем дефолтный конфиг в тестовую папку (и создаем в ней файл тест) - 
sudo cp default test

открываем скопированный файл с настройками - 
sudo nano test

отредактируем эти настройки, удалаем лишнее - 

listen 80;

listen [::]:80;

в качестве дирректории по умолчанию назначаем тестовую папку - 
root /var/www/html/test;

главным файлом укажем index.php - 
index index.php;

назначаем тестовый домен по умолчанию - 
server_name test;

для корректной работы FastCGI раскомментирую следующие строки - 

location ~\.php$ {

	include......
	
	fastcgi_pass unix:/run/php/php8.1-fpm.sock; (меняем версию php)
	
}

location ~ /\.ht {

	deny all;
	
}

сохранаем конфигурацию и выходим.

Заходим в дирректорию - 
cd ../sites-enabled/

добавим символьную ссылку к нашему домену следующей командой - 
sudo ln -s /etc/nginx/sites-available/test /etc/nginx/sites-enabled/

перезагрузаем сервер - 
sudo service nginx reload
sudo service nginx restart

Добавим файл в хост, открываем хост в нано - 
sudo nano /etc/hosts

Добвим настройку для тестовой папки - 
127.0.1.1	test

сохраняем и выходим

запускаем в браузере и получаем результат - 
http://test/ - Hello World!


	Установка MySQL

установка сервера mysql - 
sudo apt-get install mysql-server

установка клиента mysql - 
sudo apt-get install mysql-client

получаем доступ, проверяем работоспособность и смотрим информацию - 

sudo mysql -u root

SHOW DATABASES;

создаем базу - 
CREATE DATABASE testdb;

заходим в таблицу - 
USE testdb;

просмотр имеющихся таблиц - 
SHOW TABLES;


