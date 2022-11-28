# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

As we will be using AWS as our cloud provider for this project, the project prequisite is AWS EC2 instance.

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
ssh -i <private-key-name>. pem ubuntu@
