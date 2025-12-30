```
sudo apt update
sudo apt install vim
sudo apt install nginx
sudo apt install ufw
sudo ufw enable

sudo ufw app list
sudo ufw allow 'Nginx HTTP'

systemctl status nginx
sudo systemctl start nginx

sudo mkdir -p /var/www/your_domain/html
sudo chown -R $USER:$USER /var/www/your_domain/html

sudo chmod -R 755 /var/www/your_domain

sudo vim /var/www/your_domain/html/index.html
```
paste this html to index.html
```
<html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain server block is working!</h1>
    </body>
</html>
```

**Setup Nginx**
```
sudo vim /etc/nginx/sites-available/cheaheang.dev
```
**Paste this code your_domain file**
```
server {
        listen 80;
        listen [::]:80;

        root /var/www/cheaheang.dev/html;
        index index.html index.htm index.nginx-debian.html;

        server_name cheaheang.dev www.cheaheang.dev;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

#### **Vagrant**
**If not yet install chocolatey**
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
**If not yet install vagrant**
```
choco install vagrant --version=2.4.3 -y

choco install virtualbox --version=7.1.4 -y
```
```
vagrant init centos/stream9
vagrant up
vagrant ssh
```

**Vagrant CMD**
```c
// enter virtual box
vagrant ssh scriptbox
```