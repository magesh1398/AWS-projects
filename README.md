# AWS-projects

#Hosting Wordpress Website on EC2 Instance.

First we can create ec2 instance through AWS console with required Operating System, Instance type, Security Group and Storage type (also we need to create or use exsisting key pair for instance).

Once above step is completed we need to assign Elastic IP to the instance because if the server gets rebooted for any reason, the IP assigned to instance will be getting changed for each reboot.

Now we can connect instance through instance connect feature through browser.(or else we can connect with third party applicatons by providing keypair).

Once Instance is created and elastic Ip is assigned, we need to install apache and wordpress on the server, which will help us to host the websites and working.
Follow below steps
 
- Install Apache server
    sudo apt install apache2

- Install php runtime and php mysql connector which helps on dynamic web application
    sudo apt install php libapache2-mod-php php-mysql

- Now install MySQL server
    sudo apt install mysql-server

- Install MySQL server
    sudo apt install mysql-server 

- Login to MySQL server through aws cli
    sudo mysql -u root

- Change authentication plugin to mysql_native_password (change the password to something strong)
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'Test123';

- Now create a new database user for wordpress (change the password to something strong)
    CREATE USER 'wp_user'@localhost IDENTIFIED BY 'Test123';

- Next create a database for wordpress
    CREATE DATABASE wp;

- Now grant all privilges on the database 'wp' to the newly created user
    GRANT ALL PRIVILEGES ON wp.* TO 'wp_user'@localhost;

- Now download wordpress
    cd /tmp
    wget https://wordpress.org/latest.tar.gz

- next unzip (if installation is needed please install unzip first)
    tar -xvf latest.tar.gz

- Move wordpress folder to apache document root
    sudo mv wordpress/ /var/www/html

Once above steps are completed we will be able to access site using link
http://<EC2_PUBLIC_IP>/wordpress

Now login wordpress site with Database name, username, password and localhost name (if you get any error with wp-config.php file missing, we can create it under /var/www/html/wordpress directory) 

Now we should be able to view the wesite.

if you don't want to show /wordpress path while accessing the site (only http://<EC2_PUBLIC_IP>), we can modify the Document line under "/etc/apache2/site-available/000-default.conf" file

- now restart/reload apache server to see the changes
sudo systemctl restart apache2
OR
sudo systemctl reload apache2

Finally we can register our wesite with doamin name by buying through website hosting service providers
and once regsterd we can update the same in Wordpress admin page settings to use domain name instead of ip addrress

again we have to update /etc/apache2/site-available/000-default.conf file with ServerName and ServerAlias
example:
  ServerName mywordpress_web.online
  ServerAlias www.mywordpress_web.online
  
Now we can assign **ssl certificate** to our website with help of below commands
  sudo apt-get update
  sudo apt install certbot python3-certbot-apache

- Request and install ssl on your site with certbot
  sudo certbot --apache
