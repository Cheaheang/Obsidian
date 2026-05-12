	 **Setup proejct**
```
sudo apt update && sudo apt upgrade -y
```
Install nginx
```
sudo apt install nginx -y
```
Install PHP 
```
# Add PHP repo
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update

# Install PHP 8.2 + important extensions
sudo apt install php8.2 php8.2-cli php8.2-fpm php8.2-mbstring php8.2-xml php8.2-bcmath php8.2-curl php8.2-mysql php8.2-zip unzip -y
```
Install Composer 
```
cd ~
curl -sS https://getcomposer.org/installer -o composer-setup.php

sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
composer -V
```
Install Node.js + npm
```
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
node -v
npm -v
```
Install MySQL
```
sudo apt install mysql-server -y
sudo mysql
```
```
CREATE USER 'root_user'@'localhost' IDENTIFIED BY 'root';

GRANT ALL PRIVILEGES ON shop_website.* TO 'root_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;

// Make your .env according to this mysql Config
```
**Config SSH & Clone**  
```
ssh-keygen -t ed25519 -C "info"
cat ~/.ssh/id_ed25519.pub

// verify 
ssh -T git@github.com

//allow permission 
cd /var
sudo chmod 777 www/

git clone git@github.com:Cheaheang/shop_website.git
git checkout dev
```

**Config Project**(laravel+Vue)
```
cp .env.example .env

composer install 
npm install

php artisan key:generate
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
php artisan migrate
php artisan storage:link
```
**Hosting**
Create file in nignx(name: shop)
```
sudo vim /etc/nginx/sites-available/shop
```
Past this config 
```
server {
    listen 80;
    server_name YOUR_IP;
	client_max_body_size 12M;
    root /var/www/shop_website/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
Enable the site 
```
sudo ln -s /etc/nginx/sites-available/shop /etc/nginx/sites-enabled/
```
Remove default site (important)
```
sudo rm /etc/nginx/sites-enabled/default
```
Test nginx 
```
sudo nginx -t
```
Edit inbound in AWS 
- Allow port 80 with my ip-address(ur laptop)
Then allow permission
```
sudo chown -R www-data:www-data /var/www/shop_website
```
Then Build the vue project 
```
npm run build
```


**Upload image problem**
Give permission
```
sudo chown -R www-data:www-data /var/www/shop_website
sudo chmod -R 775 /var/www/shop_website/storage
sudo chmod -R 775 /var/www/shop_website/bootstrap/cache
```
add temp directory 
```
sudo vim /etc/php/8.2/fpm/php.ini
```
make sure in php.ini have this 
```
file_uploads = On
upload_tmp_dir = /tmp
```
**Stop instance(EC2) then start again and it didn't show the project**
Disable apache and start nginx 
```
sudo systemctl stop apache2
sudo systemctl disable apache2

sudo systemctl start nginx
```

**Config Domain**
- namecheap
In namecheap:  Go to **Domain List** -> Manage(Your domain) -> **NAMESERVERS**(SECTION) : Seelct **Namecheap BasicDNS ** -> **Advance DNS** -> HOST RECORDS(SECTION) : put ur ip(EC2) to both HOST(@, www)
- nginx
change server name 
```
server_name 13.54.61.118;
```
to
```
server_name cheaheang.dev www.cheaheang.dev;
```

**Enable HTTPS** 
```
sudo apt update
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d cheaheang.dev -d www.cheaheang.dev
```
clear old dns
```
ipconfig /flushdns
```
**Tips:**
Check domain attach to ip
```
nslookup cheaheang.dev
```