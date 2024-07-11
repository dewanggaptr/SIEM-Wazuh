Install packager bawaan
```
sudo apt update
sudo apt install -y curl wget gnupg2 ca-certificates lsb-release apt-transport-https
```
Add repositori ke sistem
```
sudo add-apt-repository ppa:ondrej/php
```
Install PHP
```
sudo apt install php8.0-fpm
php -v
```
Menambahkan extension PHP (opsional)
```
sudo apt install php8.0-mysql php8.0-xml php8.0-mbstring
```

Integrasi Nginx dengan PHP
```
sudo nano /etc/nginx/nginx.conf
```
rubah user `nginx` menjadi `www-data`, lalu edit file web server nginx
```
sudo nano /etc/nginx/conf.d/default.conf
```
Rubah seperti ini
```
location / {
  root /usr/share/nginx/html;
  index index.php index.html index.htm;
  try_files $uri $uri/ /index.php?$args;
}

location ~ \.php$ {
  # root html;
   fastcgi_pass unix:/run/php/php8.0-fpm.sock;
   fastcgi_index index.php;
   fastcgi_param SCRIPT_FILENAME /usr/share/nginx/html$fastcgi_script_name;
   include fastcgi_params;
}
```
Cek konfigurasi nginx dan restart service
```
sudo nginx -t
sudo systemctl nginx restart
```

Cek Integrasi PHP dengan Nginx
```
sudo echo "<?php phpinfo(); ?>" | sudo tee /usr/share/nginx/html/index.php
```
lalu akses `http://ipaddress/index.php`
