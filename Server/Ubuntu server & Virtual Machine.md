
- Setup Virtual machine (ubuntu server)
```
// setup root password
passwd root 
// give permission to folder
sudo chmod -R 777 /path/to/your/folder
```
- Installation in ubuntu server
```
sudo apt update
sudo apt upgrade -y

// installation
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx



```
- Test Web Server
```
ip a
```
- Install PHP
```
sudo apt install php php-fpm php-mysql php-xml php-mbstring php-curl php-zip unzip -y
php -v

sudo apt install composer -y
composer --version
```




job for apache2.service failed because the control process exited with error code. 
x apache2.service - the apache http server 
active: failed