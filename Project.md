# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

Project Prequisite is AWS EC2 instance.

## Steps Involved
1.   INSTALLING APACHE AND UPDATING THE FIREWALL
2.   INSTALLING MYSQL
3.   INSTALLING PHP
4.   CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
5.   ENABLE PHP ON THE WEBSITE

## Implementation
Since we are using the Ubuntu AMI of AWS EC2 instance to host our application. On our terminal, we will change directory to the folder we have our .pem file. In this case, it is in the Downloads folder

```
cd ~/Downloads
```

Now, we will change permissions for the private key file (.pem), otherwise we will get the error “Bad permission”

```
sudo chmod 0400 <private-key-name>.pem
```

We will then connect to the instance by running 

```
ssh -i <private-key-name>. pem ubuntu@<Public-IP-address>
```

## INSTALLING APACHE AND UPDATING THE FIREWALL
Now, we will be installing Apache using Ubuntu’s package manager ‘apt’:
```
#Update a list of packages in package manager
sudo apt update

#Run apache2 package installation
sudo apt install apache2

```
To verify that apache2 is running as a Service in our OS, we will use the following command
```
sudo systemctl status apache2

```
The result gotten should look like this

![Apache Verification](https://github.com/Omolade11/LampStack_AWS/blob/main/Images/Screenshot%202022-11-28%20at%2010.14.44.png "Apache Verification")

This means that we did everything correctly. Our server is running and we can access it locally and from the Internet. To access it locally, we will run the following:
```
curl http://localhost:80
or
 curl http://127.0.0.1:80
```
The 2 commands above actually do pretty much the same – they use ‘curl’ command to request our Apache HTTP Server on port 80 (actually you can even try to not specify any port – it will work anyway). The difference is that: in the first case we try to access our server via DNS name and in the second one – by IP address (in this case IP address 127.0.0.1 corresponds to DNS name ‘localhost’ and the process of converting a DNS name to IP address is called "resolution").

We will get strangely formatted text as output
![content of apache](https://github.com/Omolade11/LampStack_AWS/blob/main/Images/Screenshot%202022-11-28%20at%2011.11.02.png "Content")

Now it is time for us to test how our Apache HTTP server can respond to requests from the Internet.

Open a web browser of your choice and try to access following url
http://<Public-IP-Address>:80
 
Another way to retrieve Public IP address, other than to check it in AWS Web console, is to use following command:
 ```
 curl -s http://169.254.169.254/latest/meta-data/public-ipv4
 ```
 Having checked through my public ip address, this is the result I got.

 ![Result](https://github.com/Omolade11/LampStack_AWS/blob/main/Images/Screenshot%202022-11-28%20at%2011.39.00.png "Result")
 
 It is the same content that we previously got by ‘curl’ command. But this time, represented in nice HTML formatting by the web browser.

 ## INSTALLING MYSQL
 
 Now that we have a web server up and running, we need to install a Database Management System (DBMS) to be able to store and manage data for our site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.
 
 ```
 sudo apt install mysql-server
 ```
 
 When prompted, confirm installation by typing Y, and then ENTER.
 
 Login to the MYSQL console by typing:
 ```
 sudo mysql
 ```
 It will connect to the MySQL server as the administrative database user root
 
![SQL](https://github.com/Omolade11/LampStack_AWS/blob/main/Images/Screenshot%202022-11-28%20at%2011.58.13.png)

 Here, we will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as Omolade.9.
 ```
 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Omolade.9'; 
```
 
 We will exit the MySQL shell with the exit command:
 
``` 
 mysql> exit 
 ```
 
Afterward, Start the interactive script by running:
``` 
sudo mysql_secure_installation
```
 
This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.
If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.
Answer Y for yes, or anything else to continue without enabling.

 If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g., PassWord.1.

 When you’re finished, test if you’re able to log in to the MySQL console by typing:
```
 sudo mysql -p
```
 
 To exit the MySQL console, type:
```
 mysql> exit
```
 Our MySQL server is now installed and secured. Next, we will install PHP, the final component in the LAMP stack.

## INSTALLING PHP
 
We have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, We’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
 
To install these 3 packages at once, we will run:
```
 sudo apt install php libapache2-mod-php php-mysql
```
 Once the installation is complete, we will run this command to confirm it
 ```
 php -v
 ```
 The result should look like this
 ![php confirmation](https://github.com/Omolade11/LampStack_AWS/blob/main/Images/Screenshot%202022-11-28%20at%2014.32.54.png)
 
 At this point, our LAMP stack is completely installed and fully operational.
 
 ## CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
 
 In this project, we will set up a domain called projectlamp (can be replaced with any domain of choice)
 
Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory. 
 
We will leave this configuration as is and will add our own directory next next to the default one.
 
We will create the directory for projectlamp using ‘mkdir’ command as follows:
 
 ```
 sudo mkdir /var/www/projectlamp
 ```
Next, we will be assigning ownership of the directory with our current system user:
 
 ```
  sudo chown -R $USER:$USER /var/www/projectlamp
```
 Then, we will create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using vim:
 
 ```
 sudo vi /etc/apache2/sites-available/projectlamp.conf
```
 This will create a new blank file. We will Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:
 ```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

 ```
 To save and close the file, we will press the esc button and thereafter type ":wq" where w for write and q is for quit. Afterward, we will click the enter button.
 
we will use the ls command to show the new file in the sites-available directory
```
 sudo ls /etc/apache2/sites-available
 
 ```
Something like this will come up;
 
```
 000-default.conf default-ssl.conf  projectlamp.conf
```
we can now use a2ensite command to enable the new virtual host:
```
 sudo a2ensite projectlamp
```
To disable Apache’s default website use a2dissite command, we will run:
```
 sudo a2dissite 000-default
```
 To make sure our configuration file doesn’t contain syntax errors, we will run:
```
 sudo apache2ctl configtest
```
 Finally, we will reload Apache so these changes take effect:
 ```
 sudo systemctl reload apache2
```
 
 Our new website is now active, but the web root /var/www/projectlamp is still empty. 
 We will create an index.html file in that location so that we can test that the virtual host works as expected:
 ```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
 ```
Now we will go to our browser and try to open your website URL using IP address:
http://< Public-IP-Address >:80
![application](https://github.com/Omolade11/LampStack_AWS/blob/main/Images/Screenshot%202022-11-28%20at%2016.07.01.png)
 
 ## ENABLE PHP ON THE WEBSITE
 
 With the default DirectoryIndex settings on Apache, the index.html file takes precedence, lets modify this and give precedence to the index.php file.

We need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

```
 sudo vim /etc/apache2/mods-enabled/dir.conf
```
 
 ```
 <IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
 
