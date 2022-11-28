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

![Apache Verification](/path/to/image.png "Text to show on mouseover")
