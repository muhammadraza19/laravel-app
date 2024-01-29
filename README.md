sudo apt update   #Update Repostiories
sudo apt install apache2  #Installed Apache2
sudo apt install php libapache2-mod-php php-mysql php-mbstring php-xml php-json  #Installed PHP latest and its Repositories
sudo apt install curl php-cli unzip   #Installed Zip and php-cli
cd ~
curl -sS https://getcomposer.org/installer -o composer-setup.php   #Downloaded composer installer
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer  #Installed Composer through php-cli
sudo apt install mysql-server   #Installed mysql-server
mysql -u root -p

# Created Database and user and assigned privileges to it
CREATE DATABASE laravel_db;
CREATE USER 'laravel_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON laravel_db.* TO 'laravel_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;

cd /var/www/html
sudo composer create-project --prefer-dist laravel/laravel your-laravel-app #create laravel project
sudo nano /etc/apache2/sites-available/your-laravel-app.conf          #create configuration file for your laravel-app
sudo a2ensite your-laravel-app.conf                                   #enable configuration file of laravel app
sudo service apache2 restart                                          #as configuration changes are done then we have to restart apache
cd /var/www/html/your-laravel-app
sudo chown -R www-data:www-data storage bootstrap/cache              #changing permission for webserver to read the files and run application smoothly         
sudo chmod -R 775 storage bootstrap/cache

cp .env.example .env
#Changed DB parameters for laravel-app
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=your_password

#Generated application-key
php artisan key:generate
#Migrate and seeding database
php artisan migrate --seed
