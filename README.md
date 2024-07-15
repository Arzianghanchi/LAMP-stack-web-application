# LAMP-stack-web-application

setting up MySQL, Apache, PHP, and configuring your web application on Ubuntu.
1. Clone Your Repository
Assuming your repository is hosted remotely, clone it into the default web root directory (/var/www/html/):
cd /var/www/html/
•	sudo git clone https://github.com/Anirban2404/phpMySQLapp.git.
 
Move code from phpMyAQLapp to html
•	mv * cd /var/www/html /

 
2. Install MySQL and Configure Databases
First, install MySQL server and client:
 

•	bash
sudo apt update
sudo apt install mysql-server mysql-client	 
 

During the installation, you'll be prompted to set a root password for MySQL. Make sure to remember this password as you'll need it later.

a. Configure MySQL:
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
Add or modify the following lines under `[mysqld]` section:
•	[mysqld]
collation-server = utf8_unicode_ci
character-set-server = utf8

Save and close the file. Then, restart MySQL service:
•	bash
sudo systemctl restart mysql

3.Create Databases and Import Data:
Assuming you have cloned the repository and located the SQL files (`movieDB.sql` and `bookDB.sql`) in `/var/www/html/mySqlDB/`, follow these steps:
Log into MySQL as root
•	Bash
mysql -u root -p
 
Create databases and set collation
•	CREATE DATABASE bookstore CHARACTER SET utf8 COLLATE utf8_unicode_ci;
•	CREATE DATABASE moviedb CHARACTER SET utf8 COLLATE utf8_unicode_ci;
•	EXIT;

Import SQL files into respective databases
•	mysql -u root -p bookstore < /var/www/html/mySqlDB/bookDB.sql
•	mysql -u root -p moviedb < /var/www/html/mySqlDB/movieDB.sql
 

Update Database Connection Details
•	Update database connection details in `bookDatabase.php` and `movieDatabase.php`:
•	bash
sudo sed -i -e 's/127.0.0.1/<<ip_address>>/g' /var/www/html/books/includes/bookDatabase.php 
sudo sed -i -e 's/127.0.0.1/<<ip_address>>/g' /var/www/html/movies/includes/movieDatabase.php
•	Replace `<<ip_address>>` with the actual IP address or hostname of your MySQL server.
 
4. Install Apache2 and PHP
 
Install Apache2 and PHP along with necessary modules:
•	bash
sudo apt install apache2 php libapache2-mod-php php-mysql

If your Apache configuration file (`000-default.conf` or any custom configuration file) does not explicitly mention a `<Directory>` block, you can still set `index.php` as the default index file using a different approach. Here’s how you can do it:
Editing Apache Configuration
a.  Set Permissions
•	Ensure Apache has permissions to access the files:
•	bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
b. Navigate to Apache Sites Configuration Directory
•	bash
cd /etc/apache2/sites-available/

c. Edit Your Virtual Host Configuration File
Open the appropriate configuration file for your website using a text editor. For example, `000-default.conf`:
•	bash
 	sudo nano 000-default.conf

   Replace `000-default.conf` with the actual configuration file name if you have a different one.

 Set `index.php` as Default
Inside the configuration file, locate the `DirectoryIndex` directive. If it's not present, you can add it. The DirectoryIndex` directive specifies the order of index files that Apache should look for when a directory is requested.
Add `index.php` as the first entry in the list (or adjust as needed):
•	DirectoryIndex index.php index.html index.htm
 
This line instructs Apache to first look for `index.php` when a directory is requested, then `index.html`, and finally `index.htm`.
d. Save and Exit
Save the changes and exit the text editor (`nano` in this case) by pressing `Ctrl + X`, then `Y` to confirm saving, and `Enter` to confirm the file name. 
e. Restart Apache
   After making changes to the configuration file, restart Apache to apply the changes:
•	bash
sudo systemctl restart apache2
Output
 
By following these steps, you should have MySQL, Apache, PHP, and your web application configured and running on your ubuntu server.

