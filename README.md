# Laravel-in-aws
Here is some commands for hosting a Laravel website in AWS Cloud

Install PHP in EC2 Instance

```bash
sudo dnf install -y php php-cli php-common php-mysqlnd php-fpm php-mbstring php-pdo php-gd
```

Now Check the Version of PHP to confirm it actually installed

```bash
php -v
```
Install some dependencies which are needed for laravel

```bash
sudo yum install nginx git unzip curl php-mbstring php-xml php-bcmath php-mysqlnd php-fpm -y --allowerasing
```

Install Composer

```bash
sudo yum install composer
```

Clone your project from github

```bash
cd /var/www/
sudo git clone https://github.com/your-repo.git laravel-app

composer install
```
copy you .env.example file 

```bash
cp .env.example .env

```

Install mysql/mariadb

```bash
sudo dnf install mariadb105
```

Checking version

```bash
mysql --version
```

Create a RDS database and connect with your Database from the EC2 instance

```bash
mysql -h dbendpoint -u admin -p
```

Change the dbendpoint to your DNS endpoint

Now configure nginx, create a file in confd folder

```bash
cd /etc/nginx/
sudo touch Laravel-app.conf
server {
    listen 80;
    server_name _;

    root /var/www/{projectname}/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/run/php-fpm/www.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;


    }
}
```

Check whether the configuration is ok

```bash
sudo nginx -t
```
Go to your laravel project folder and check the ownership

```bash
ls -a
```
change two folder ownership

```bash
sudo chown -R nginx:nginx /var/www/laravel-app/storage 
sudo chmod -R 775 /var/www/laravel-app/storage 
sudo chown -R nginx:nginx /var/www/laravel-app/bootstrap
sudo chmod -R 775 /var/www/laravel-app/bootstrap
```

For generating key for your project 
```bash
php artisan key:generate
```
Install nodejs for breeze

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install --lts
node -v
npm install
npm run dev
```



Now copy the IP address on the EC2 instance and paste on browser. Here you can configure your Laravel Website.

Thank you





