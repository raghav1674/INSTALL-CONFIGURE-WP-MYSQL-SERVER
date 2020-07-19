# INSTALL-CONFIGURE-WP-MYSQL-SERVER


## HOW TO SETUP WORDPRESS AND EXTERNAL DATABASE ?


      I HAVE USED UBUNTU AMI FOR BOTH THE SETUP:

>ubuntu/images/hvm-ssd/ubuntu-bionic-18.04-amd64-server-20200611 (ami-02d55cb47e83a99a0)

<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/01_wp.PNG">

<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/02_wp.PNG">
<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/03_wp.PNG">




### 1. CREATE AN APPLICATION SERVER:
  
      On the application server, we will install Apache to serve the HTTP requests and PHP to interpret PHP code requested by Apache. And we are going to use the MySQL database       hosted on the database server, so we do not have to install MySQL on this server.
<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/04_wp.PNG">

 ```
 get update
 apt-get upgrade -y
 apt-get install apache2 -y
 apt-get install php7.2 php7.2-curl php7.2-mysql php7.2-mbstring php7.2-dom php7.2-gd -y
 apt-get install libapache2-mod-php7.2
 apt-get install mysql-client -y 
 ```

 
/*
To verify if Apache and PHP are installed and configured correctly, execute the following commands and then access the public IP address of the application server in the browser.

 rm /var/www/html/index.html
 echo “<?php phpinfo(); ?>” >> /var/www/html/index.php 
 */
 
 <img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/05_wp.PNG">
 <img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/06_wp.PNG">
 <img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/07_wp.PNG">


### 2.CONFIGURE DATABASE SERVER:
<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/01_sql.PNG">
<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/02_sql.PNG">
<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/03_sql.PNG">
<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/04_sql.PNG">


 ```
 apt-get update
 apt-get upgrade -y
 apt-get install mysql-server mysql-client -y
 mysql_secure_installation
 ```



#### While running the fourth command, You have to answer a few questions. Answer it like this.

##### Enable Validate Password Plugin? No
##### SET THE PASSWORD
##### Change the password for root? No
##### Remove anonymous users? Yes
##### Disallow root login remotely? Yes
##### Remove test database and access to it? Yes
##### Reload privilege table now? Yes

/*
 mysql -uroot -p
 Then type the password you have set .

   If you can log into your MySQL server, you have successfully installed MySQL on the server. 
 
 exit 
 */


```
vi /etc/mysql/mysql.conf.d/mysqld.cnf

edit this:>>  bind-address = 0.0.0.0 or (Instance Private IP)

service mysql restart
```
<img src="https://github.com/raghav1674/INSTALL-CONFIGURE-WP-MYSQL-SERVER/blob/master/SETUP/05_sql.PNG">


### 3.DOWNLOAD WORDPRESS ON THE APPLICATION SERVER:


```
cd /var/www/html
 rm -rf *
 wget https://wordpress.org/latest.tar.gz
 tar -xzvf latest.tar.gz
 mv wordpress/* ./
 rm -rf wordpress latest.tar.gz
 chown -R www-data:www-data /var/www/html
 ```


now you can create snapshot and then create ami of both wordpress and mysql and run the following command after the whole setup of task3 :

### CREATE DATABASE FOR WORDPRESS ON DATABASE SERVER:


```
mysql -uroot -p;
Then type the password you have set .

mysql> CREATE DATABASE wordpress;
mysql> CREATE USER ‘wordpressUser‘@’PRIVATE_IP_OF_WORDPRESS‘ IDENTIFIED BY ‘PASSWORD‘;
mysql> GRANT ALL PRIVILEGES ON wordpress.* TO ‘wordpressUser‘@’PRIVATE_IP_OF_WORDPRESS‘;
mysql> FLUSH PRIVILEGES;
```


/*
Now, if you want to test if the user and database are successfully created, login to your application server and execute the following command to log in to your MySQL server from your application server.

mysql -uwordpressUser -p -h<PRIVATE_IP_OF_MYSQL_INSTANCE>
Then type the password you have set WHILE CREATING USER: ie PASSWORD.

exit  
*/



### INSTALL WORDPRESS FROM BROWSER

```
TYPE THE PUBLIC IP OF THE WORDPRESS INSTANCE TO ACCESS IT .
Enter the following details as per you have set while creating database and user in sql for wordpress:

TYPE THE DATABASE NAME WHICH WE HAVE CREATED: eg; wordpress
TYPE THE USERNAME ; eg; wordpressUser
TYPE THE PASSWORD, eg; PASSWORD
TYPE THE DATABASE HOST , (PRIVATE_IP_OF_MYSQL_INSTANCE)

```

####  YOU HAVE SUCCESSFULLY INSTALLED AND CONNECTED WORDPRESS WITH EXTERNAL DATABASE







