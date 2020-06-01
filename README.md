#vWhat is Phalcon?

Phalcon is a php extension written in C. The code is pre-compiled and not interpreted which improves performance. The Zephir/C extension and PHP are loaded together on server startup which makes the functions and classes available to all php applications.

Links:
https://zephir-lang.com/en
https://en.cppreference.com/w/
https://www.php.net/


## Phalcon pros 🙂

* High performance
* PSR-4 autoload compliant - https://www.php-fig.org/psr/psr-4/
* Command line devtools - https://phalcon.io/en-us/download/tools
* Active Community - https://phalcon.io/en-us/about
* Minimal library including components like MVC - https://github.com/phalcon/cphalcon/tree/master/phalcon
* Started 4th November 2012 (mature)
* Templating (VOLT), ORM and Routing included - https://docs.phalcon.io/4.0/en/volt && https://en.wikipedia.org/wiki/Object-relational_mapping

## Phalcon cons 🙁

* Installation instructions outdated
* Extension compilation can be tricky

## Tutorial Requirements:

* MacOS >= 10.14 - https://support.apple.com/en-us/HT210190
* MAMP 5.7 - https://www.mamp.info/en/mamp/mac/
* Apache 2.2.34 VirtualHost configuration - http://apache.org/
* MySQL 5.7 create a database - https://dev.mysql.com/doc/refman/5.7/en/mysql-nutshell.html
* PHP 7.2 - https://www.php.net/releases/7_2_0.php
* Composer installs devtools - https://getcomposer.org/
* Phalcon - https://phalcon.io/en-us/download/linux

## We’ll cover:

* Installation of MAMP & downgrade to php 7.2
* Installation of phalcon.so
* Installation of phalcon devtools
* Localhost host configuration
* Apache VirtualHost server configuration
* Database creation
* Scaffolding an application with devtools
* Verify the application in browser

⚠ Article written in 2014 and refreshed in 2020. Development environments will differ. For guidance, see original install docs for phalcon & devtools. Getting stuck, drop a comment.

* https://phalcon.io/en-us/download/linux
* https://github.com/phalcon/phalcon-devtools


## 1. Installation of MAMP & downgrade to php 7.2 Download and install MAMP

###Downgrade to php7.2. Edit file: /Applications/MAMP/conf/apache/httpd.conf Overwrite 7.4.2 with 7.2.22

LoadModule php7_module /Applications/MAMP/bin/php/php7.4.2/modules/libphp7.so

⚠ As no precompiled php7.4 phalcon.so extension exists in the php-phalcon-mamp repo downgrade MAMP’s from php7.4 to php7.2.

### Stopping Apache:

sh /Applications/MAMP/bin/stopApache.sh

###Starting Apache:

sh /Applications/MAMP/bin/startApache.sh

⚠ Do not use MAMP GUI buttons, these will undo and overwrite changes, because MAMP wants everyone to use MAMP PRO. Instead control the server using the provided bash commands. Or depending on your CLI familiarity locate the MAMP apachectl. If you’re a beginner stick with the bash scripts.

### Verify successful downgrade by checking phpinfo and search for ‘Configuration file path‘ and ‘Loaded configuration file‘ both should reference php7.2.22.

## 2. Installation of phalcon.so

### cd to the ‘~’ user directory

cd ~

### Clone the phalcon mamp extensions

git clone https://github.com/majksner/php-phalcon-mamp

### cd into the right php extension directory

cd php-phalcon-mamp/php7.2.1

### cp the phalcon.so extension to the php extension directory

cp ./phalcon.so /Applications/MAMP/bin/php/php7.2.22/lib/php/extensions/no-debug-non-zts-20170718/

### Find and edit the active ‘loaded’ php.ini. Edit file:

/Applications/MAMP/bin/php/php7.2.22/conf/php.ini

### Search for text:

; Dynamic Extensions ;

### Add your phalcon.so beneath the last entry and save the file

extension=phalcon.so

### Stop and Start Apache using the bash method in Section 1 (above). Navigate to phpinfo. On the page search for ‘phalcon’.

## 3. Installation of phalcon devtools via composer

### cd back to user home.

cd ~

💡 If you’ve followed the tutorial you could also use cd –

### On the command line type.

which composer

### The above should output an absolute path to composer. If it does not run the following command to get composer.

curl -s http://getcomposer.org/installer | php

⚠ If you have any issues refer to the composer installation and install locally or globally. A global installation is more maintainable on a personal machine.

### Once composer is installed, type composer on the command line which will output a list of available commands.

php composer.phar

⚠ The best composer setup is to use a symlink to composer where php directly is not directly invoked.

### Once you have a composer available. create the composer.json to install phalcon devtools.

vi composer.json

### Type

{
    "require-dev":
        {
            "phalcon/devtools": "^3.4"
        }
}

### Then

/Applications/MAMP/bin/php/php7.2.22/bin/php composer.phar install

⚠ depending on your .bash_profile, and $PATH environment variable composer may be invoked under a different name i.e. plain old “composer” this is cleaner.

### After devtools installation via composer type the command below to verify

/Applications/MAMP/bin/php/php7.2.22/bin/php ./vendor/bin/phalcon -v

⚠ Your phalcon devtools are not available to system or your .bash_profile which is why ./vendor directory is used.

## 4. Localhost host configuration

### Open your hosts file, edit as root, tip ‘sudo nano’:

/private/etc/hosts

💡 If you have trouble use a hosts editor similar to HostMan to simplify.

### Append the comment and host entry below to the end of the file

127.0.0.1 phalcon

### Test the new entry. The command below should not timeout.

ping -c 4 phalcon

💡 If you have trouble disable ‘stealth mode’, System Preferences -> Security & Privacy -> Firewall -> Firewall options -> Enable stealh mode . Retry.

## 5. Apache VirtualHost server configuration

### Clone the repo under your htdocs

git clone https://github.com/colin-humphrey/phalcon.git

### Add a VirtualHost configuration entry for the phalcon site

<VirtualHost *:80>
    ServerAdmin admin@phalcon
    DocumentRoot "/Applications/MAMP/htdocs/phalcon/public"
    ServerName phalcon
    ErrorLog "/Applications/MAMP/logs/phalcon-error_log"
    CustomLog "/Applications/MAMP/logs/phalcon-access_log" common
    <Directory "/Applications/MAMP/htdocs/phalcon/public">
        DirectoryIndex index.php
    </Directory>
</VirtualHost>

### Test the paths and verify nothing broken, the command should output. Syntax OK

/Applications/MAMP/bin/apache2/bin/httpd -t

## 6. Database creation

### Navigate to phpmyadmin SQL editor and create the database

CREATE DATABASE phalcon;

### Select the newly created phalcon database and create a table against it.

CREATE TABLE `basics` (`id` int not null auto_increment primary key,`fact` varchar(255),`pro` varchar(255),`con`varchar(255),`source`varchar(255),`description` varchar(255),`notes` text,`created` timestamp null default null,`modified` timestamp not null default current_timestamp on update current_timestamp )

## 7. Scaffolding application with devtools

### Once the table is created on the database. Navigate to the phalcon DocumentRoot:

cd /Applications/MAMP/htdocs/phalcon/

### Use the phalcon command to scaffold your app

phalcon create-project phalcon

### Edit the database configuration and ensure the username, password and database are correct. - Edit app/config/config.php

'username' => 'root', 'password' => 'root', 'database' => 'phalcon'
// MAMP defaults are 'root' and 'root'
The baseUri should be “/” -> 'baseUri' => '/'

### Scaffold the DB 

phalcon scaffold basics

### Restart the server using the bash scripts as mentioned in Section 1. above.

## 8. Verify application in browser

### Navigate to

http://phalcon/basics 🎉
