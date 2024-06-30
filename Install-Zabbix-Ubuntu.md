```
#!/bin/bash 
```
# Установка nginx 
```
sudo apt-get install nginx -y 
 
sudo apt-get update 
 
sudo systemctl enable nginx 
```
 
# Установка PostgreSQL 
``` 
sudo apt install -y postgresql-common 
 
sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh 
 
sudo apt-get update 
```
# Установка Zabbix 
```
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu24.04_all.deb 
 
dpkg -i zabbix-release_6.4-1+ubuntu24.04_all.deb 
 
sudo apt-get update 
 
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent 
```
# Создание базы данных  
```
sudo -u postgres createuser --pwprompt zabbix 
 
sudo -u postgres createdb -O zabbix zabbix 
 
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix 
```
# Изменение сервера на ваш и пароля 
```
sudo sed -i 's/DBHost=localhost/DBHost=10.10.10.10/g' /etc/zabbix/zabbix_server.conf 
 
sudo sed -i 's/DBPassword=/DBPassword=123456/g' /etc/zabbix/zabbix_server.conf 
```
# Перезапуск Zabbix сервера 
```
systemctl restart zabbix-server zabbix-agent nginx php8.3-fpm 
 
systemctl enable zabbix-server zabbix-agent nginx php8.3-fpm
```
