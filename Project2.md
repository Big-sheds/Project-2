## STEP 1 : INSTALLING THE NGINX WEB SERVER 

  **NGINX is a high-perfomance web server. We install the webServer using `apt`**

`$sudo apt update`

`$sudo apt install nginx`

![Install NGINX](./Images-2/image-2.1.jpg)



**When Prompted, enter `Y` to confirm Installation.**

**To verify that NGINX was successfully install run ;**

`$sudo systemctl status nginx`

![NGINX Status](./Images-2/image-2.2.jpg)

**If it's green and running then, we're good**

**We have just launched our first web Server in the Cloud**


**But before we can receive any traffic by our Web Server, we need to open `TCP port 80` that is the default port web browers use to access web pages in the internet.**

![NGINX Status](./Images-2/image-2.3.jpg)


**Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).**

 **We can access it locally in our Ubuntu shell by running:**

 `$ curl http://localhost:80`

  or

  `$ curl http://127.0.0.1:80`


  **We have to test how our Nginx server can respond to requests from the Internet. Open a web browser of your choice and try to access the url using**


  `http://<Public-IP-Address>:80`

  ![NGINX Status](./Images-2/image-2.4.jpg)


## STEP 2 — INSTALLING MYSQL

**Our webserver is up and running. We need a Database Management System(DBMS) to store and manage data for our site in a relational database MYSQL**


**To install, we `apt`**

`$ sudo apt install mysql-server`

  ![NGINX Status](./Images-2/image-2.5.jpg)

  **We have to login to MYSQL server console by typing:**

  `$ sudo mysql`

  
  ![NGINX Status](./Images-2/image-2.6.jpg)

  **It's recommended to run a security script that comes pre installed with MYSQL. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method**


  `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

  **Exit the MySQL shell with:**

  `mysql> exit`

  **To start the interactive script we run;**

  `$ sudo mysql-secure-installation`


  ## STEP 3 – INSTALLING PHP
**WE install PHP to process code and generate dynamic content for the web server**

**To install we run;**

**NOTE:**
While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.


`Sudo apt install php-fpm php-mysql`

  ![NGINX Status](./Images-2/image-2.7.jpg)


## STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR

 WHEN USING NGINX WEB SERVER, WE CAN CREATE SERVER BLOCKS.
 *The Nginx server block allows us to run multiple websites on a single machine.*

 **We will use projectLEMP: On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.**

 **To create the root web directory for your_domain we run;**

 `sudo mkdir /var/www/projectLEMP`

 We have to assign ownership of the directory with the $USER environment variable.

 **We run;**

 `sudo chown -R $USER:$USER /var/www/projectLEMP`

 **To open a new configuration file in Nginx's sites-available directory using nano we run;**

 `sudo nano /etc/nginx/sites-available/projectLEMP`

 **We paste the command in the blank file**

 #/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}


**We activate our configuration by linking the config. file fron Nginx sites-enabled directory**

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`


*Test for syntax errors by typing*

`sudo nginx-t`


  ![NGINX Status](./Images-2/image-2.8.jpg)


**WE need to disable default Nginx host thats is currently configured to listen on port 80**

`sudo unlink /etc/nginx/sites-enabled/default.`

**Reload Nginx to apply changes**

`sudo sysytemcl reload nginx`