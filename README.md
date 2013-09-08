Symfony Standard Edition + Draggy
=================================

Draggy is a code development tool and template engine that enables the user to create and maintain a functional
Skelleton of an application.

On this bundle you can download the current Symfony2 Standard Edition with Draggy bundled onto it. It is the same as
if you downloaded the [Symfony Standard Edition](https://github.com/symfony/symfony-standard) and then followed
the Draggy installation steps found as described [here](https://github.com/j-d/draggy).

If you want to start with a sample working demo, please follow the steps on the [Draggy demo](https://github.com/j-d/draggy-demo).

Installation (if you know what you are doing)
---------------------------------------------

Install composer (You can find how to install Composer here: [Composer](http://getcomposer.org/doc/00-intro.md))
   
``` 
cd
curl -s https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```
```
composer create-project jd/symfony-standard-draggy /path/to/webroot/ master
```

Installation (for dummies)
--------------------------
Make sure you have all the necessary dependencies installed:

```
sudo apt-get install git apache2 php5 php5-sqlite php-apc php5-intl mysql-server php5-mysql phpmyadmin php-apc php5-intl curl
```

Install composer (You can find how to install Composer here: [Composer](http://getcomposer.org/doc/00-intro.md))
   
``` 
cd
curl -s https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```
```
composer create-project jd/symfony-standard-draggy /var/www/draggy/ master
```

If it is the first Symfony project you install on the machine, is going to take a few minutes.
It will eventually ask you to configure the parameters. Just accept the default values, but make sure that 
the MySQL password `database_password` is correct.

Next, create the database for the project:

```
cd /var/www/draggy/

php app/console doctrine:database:create
```

Create the virtual host on Apache called `draggy.local`

```
echo date.timezone = Europe/London | sudo tee -a /etc/php5/cli/php.ini
echo date.timezone = Europe/London | sudo tee -a /etc/php5/apache2/php.ini
echo short_open_tag = Off | sudo tee -a /etc/php5/cli/php.ini
echo short_open_tag = Off | sudo tee -a /etc/php5/apache2/php.ini

echo 127.0.0.1 draggy.local | sudo tee -a /etc/hosts

sudo tee /etc/apache2/sites-available/draggy << EOF
<VirtualHost *:80>
	DocumentRoot "/var/www/draggy/web"
	DirectoryIndex app.php
	ServerName draggy.local
	<Directory "/var/www/draggy/web">
		AllowOverride All
		Allow from All
	</Directory>
</VirtualHost>
EOF

sudo a2ensite draggy
sudo service apache2 reload

sudo a2enmod rewrite
sudo service apache2 reload
```

That's it! To use it just browse to the path you created http://draggy.local/app_dev.php/_draggy

Psst! Remember to create the Bundles before generating the source code:

```
cd /var/www/draggy/
php app/console generate:bundle --format yml
```

And that if you update the schema, you will need to ask Doctrine to create or update it:

```
php app/console doctrine:schema:update --force
```

Happy Draggy-ing!
