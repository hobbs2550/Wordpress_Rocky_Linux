# Wordpress_Rocky_Linux
Describes how I downloaded Word Press to Rocky Linux

#The following are steps that I took to download WordPress to Rocky Linux 8
1. Update DNF repository cache + install wget
   sudo dnf update
     ![alt text] (<img width="625" alt="Screenshot 2023-12-06 at 11 32 48 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/6536d265-a572-4295-8294-b8acef2160b1">)
   sudo dnf install wget nano -y
     ![alt text] (<img width="613" alt="Screenshot 2023-12-06 at 11 33 29 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/d3615ba7-23b2-4891-9471-af57d3a8b163">)

2. Installing Apache Web Server + Enabling the web server
    sudo dnf install httpd -y
     ![alt text] (<img width="648" alt="Screenshot 2023-12-06 at 11 35 47 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/ebe0a47b-a034-4871-9792-7d5b13b307cb">)
    sudo systemctl enable --now httpd
   systemctl status httpd 
    ![alt text] (<img width="1376" alt="Screenshot 2023-12-06 at 11 39 20 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/e08c1875-6d23-41db-86aa-71d3106bae8d">

3. Enable rewrite module + puuting the following at the end of the file
   sudo nano /etc/httpd/conf/httpd.conf
   LoadModule rewrite_module modules/mod_rewrite.so
   ![alt text] (<img width="551" alt="Screenshot 2023-12-06 at 11 43 04 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/56ec005e-da5a-4135-a773-bddf6e4df78f">)

4. Install Maria DB
   sudo dnf install mariadb
   ![alt text] (<img width="680" alt="Screenshot 2023-12-06 at 11 44 08 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/101feb83-3f53-4fa8-a9b3-f3bdda300e66">
    sudo systemctl enable --now mariadb
    systemctl status mariadb
   ![alt text] (<img width="1007" alt="Screenshot 2023-12-06 at 11 46 12 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/d2bc8b4e-55df-4fdc-b302-a027b19c46a4">)
   sudo mysql_secure_installation
   ![alt text] (<img width="585" alt="Screenshot 2023-12-06 at 11 48 09 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/c50c8768-79e4-4511-a109-c1b5f8e5b9d4">)

5. Installing php and restarting
    sudo dnf install php-gd php-soap php-intl php-mysqlnd php-pdo php-pecl-zip php-fpm php-opcache php-curl php-zip php-xmlrpc wget
    ![alt text] (<img width="1703" alt="Screenshot 2023-12-06 at 10 57 26 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/c36ef8a1-016d-416f-88db-7327ebf9236e">)
    sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm
    ![alt text] (<img width="639" alt="Screenshot 2023-12-06 at 11 49 59 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/9e384669-21c9-49e6-9d4c-12b449f05f7e">)
    sudo dnf update -y
    ![alt text] (<img width="639" alt="Screenshot 2023-12-06 at 11 49 59 PM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/e500bf03-34fb-49a4-93b3-20e7a23d2860">
    sudo dnf module reset php -y
    sudo dnf module enable php:remi-8.0 -y
    sudo dnf update -y
    sudo systemctl restart httpd

6. Create databases for WordPress in MariaDB
    sudo mysql -u root -p
    CREATE DATABASE wordpress_db;
    CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'complicated pw';
    GRANT ALL ON wordpress_db.* TO 'wordpress'@'localhost';
    FLUSH PRIVILEGES;
    EXIT;

7. Download WordPress in Rocky Linux
    wget https://wordpress.org/latest.tar.gz -O wordpress.tar.gz]
    tar -xvf wordpress.tar.gz
    sudo cp -R wordpress /var/www/html/

8. Set ownership + Permissions on WordPress
    sudo chown -R apache:apache /var/www/html/wordpress
    sudo chmod -R 775 /var/www/html/wordpress
    sudo semanage fcontext -a -t httpd_sys_rw_content_t "/var/www/html/wordpress(/.*)?"
    sudo restorecon -Rv /var/www/html/wordpress

9. Installing the Semenage tool
    sudo dnf whatprovides /usr/sbin/semanage. 
    sudo dnf install policycoreutils-python-utils

11. Create an Apache Configuration File for WordPress
   sudo vim /etc/httpd/conf.d/wordpress.conf
  I typed the following into the file
![alt text] (<img width="456" alt="Screenshot 2023-12-07 at 12 01 41 AM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/a9defcc8-9d65-4d93-95c6-60dbee0c2f8c">

12. Seeing if the webserver is running + changing the firewall rules.
    ![alt text] (<img width="1380" alt="Screenshot 2023-12-07 at 12 03 48 AM" src="https://github.com/hobbs2550/Wordpress_Rocky_Linux/assets/145304583/01de9c3f-807c-4272-8afe-7b383e47e351">
    sudo firewall-cmd --permanent --zone=public --add-service=http
    sudo firewall-cmd --permanent --zone=public --add-service=https
    
13. Set up WordPress
    ##The following are links that I used to understand how I took the steps above
    - https://www.youtube.com/watch?v=iCfAuPPrr04&t=169s
    - https://linux.how2shout.com/how-to-install-wordpress-on-almalinux-8-rocky-linux-8/
    - https://www.tecmint.com/install-wordpress-on-rocky-linux/
    -  https://ciq.com/blog/how-to-install-wordpress-on-rocky-linux/
    - https://www.server-world.info/en/note?os=Rocky_Linux_8&p=httpd2&f=4
    - https://www.youtube.com/watch?v=MjmHm5ej2T0
