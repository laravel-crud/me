### Step 0 — update your system, Install prerequisites
You should first update your system by running :

```
sudo apt update
```
Before anything you should install git, curl, wget, … in a fresh ubuntu 18.04:

sudo apt install -y git curl wget zip unzip
I put -y in the above command to answer all the question “yes” by default!

Step 1 — Install Apache:
sudo apt install apache2
To make sure that the server is running check the apache2 status:

sudo systemctl status apache2
You will see this:


and then press “q” to quit this session.

Step 2 — Adjust the Firewall to Allow Web Traffic
sudo ufw allow in "Apache Full"
The response will be:


Then check this address in the browser : http://your_server_ip . In my case:

http://127.0.0.1
You will see this:


As you see above, the service appears to have started successfully, you can also access to your server through the http://localhost address and you will see the same Apache2 default home page.

Attention: Enabling mod_rewrite
We need to activate mod_rewrite. It's available but not enabled with a clean Apache 2 installation.

sudo a2enmod rewrite
sudo systemctl restart apache2

Read Note 0 at the end.

Step 3— Install MySQL:
sudo apt install mysql-server
Optional:

Then remove some dangerous defaults after installing mysql:

sudo mysql_secure_installation
Think about a strong password for user root in Mysql ( in level 1=Medium, it must have sing, digit, lowercase and uppercase letters). Don’t forget to save it.

Type your selected root password when it asks. Read more about improving MySQL installation security here.


And then answer the other questions arise with Yes!

Remove anonymous users? y
Disallow root login remotely? y
Remove test database and access to it? y
Reload privilege tables now? y
Then you should try to connect mysql with root password

sudo mysql -u root -p
and when you see mysql> you can type exit to quit from mysql environment.

Step 4— Install PHP:
sudo apt install php libapache2-mod-php php-mysql
For Laravel installation and also phpmyadmin you will need some important php modules, so do this:

sudo apt install php7.2-common php7.2-cli php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-mbstring php7.2-bcmath php7.2-imap php7.2-xml php7.2-zip
Why these modules? Read more here.

Step 5— Tell the web server to prefer PHP files over others, so make Apache look for an index.php file first.
sudo nano /etc/apache2/mods-enabled/dir.conf
Before editing:


Then edit the dir.conf file in a way that index.php has the priority over the others, as like as:

<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
Then Ctrl+x and answer yes to override the file. Then

sudo systemctl restart apache2
Test:
Check the correctness of installation by:

sudo nano /var/www/html/info.php
And put this:

<?php
phpinfo();
?>
Then check in the browser this: http://your_server_ip/info.php

In my case is :

http://127.0.0.1/info.php
I will see this:


Do not forget to delete info.php file!

Step 6— Install composer
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
# this make the composer executable -> 
sudo chmod +x /usr/local/bin/composer
# check version
composer --version

Step 7 — Install Fresh Laravel Project
Change your directory to the place you want like:

cd ~
composer create-project --prefer-dist laravel/laravel my_linux_app
Then go to the project directory :

cd my_linux_app
and then start the project:

php artisan serve
Read Note 5 at bellow.

Step 8 — Verify Laravel Installation:
I will see the project in this address:

http://127.0.0.1:8000
