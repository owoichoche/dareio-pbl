# Project2.md
Jonathan Alli's 2nd project
Jonathan's 2nd project

# Project 2 - WEB stack implementation (LEMP STACK)

## Connected to EC2 instance via GitBash

I spun a new EC2 instance, downloaded and installed GitBash on my PC and connected to the instance via GitBash.

from the screenshot below, notice that I changed the directory to the directory where the **.pem** file was stored. 
![image](https://user-images.githubusercontent.com/36407447/115096773-770b5a00-9f1e-11eb-8f59-daf5af9982b6.png)

##Installed the Nginx web server

I installed the Nginx web server by using the following command:
> sudo apt update 
> sudo apt install nginx
![image](https://user-images.githubusercontent.com/36407447/115096796-8e4a4780-9f1e-11eb-9da9-81940f777c6f.png)
I also added a rule to the EC2 configuration to open inbound connection through port 80 and then, was now able to access the web server.

#### Access to the web server via the Ubuntu shell
![image](https://user-images.githubusercontent.com/36407447/115096805-a6ba6200-9f1e-11eb-97d6-98c8e449a009.png)

#### Access to web server via the browser
![image](https://user-images.githubusercontent.com/36407447/115096822-c18cd680-9f1e-11eb-9e10-a8c1e28a0eaf.png)

## Installed MySQL as confirmed below
![image](https://user-images.githubusercontent.com/36407447/115096845-e5e8b300-9f1e-11eb-913f-55190f5c059e.png)

## Installed PHP by using the command - sudo apt install php-fpm php-mysql
![image](https://user-images.githubusercontent.com/36407447/115096870-044eae80-9f1f-11eb-8f27-eb3c49a46784.png)

##Configured Nginx to use the PHP processor
First, I created a directory for the new Project. i.e. ProjectLEMP with this command ***$ sudo mkdir /var/www/projectLEMP***

Next, I assigned ownership of the directory with the $USER environment variable by entering the following command:
 - **$ sudo chown -R $USER:$USER /var/www/projectLEMP
On using the echo command, it resulted in the following:
![image](https://user-images.githubusercontent.com/36407447/115097037-d28a1780-9f1f-11eb-8ab8-466230e8e53e.png)

###tested out PHP with Nginx
![image](https://user-images.githubusercontent.com/36407447/115097066-f3526d00-9f1f-11eb-964f-f8bb5b87ddf8.png)

## created and retrived data from a new database I just created
![image](https://user-images.githubusercontent.com/36407447/115097121-344a8180-9f20-11eb-921b-37daefe0d678.png)
![image](https://user-images.githubusercontent.com/36407447/115097131-44faf780-9f20-11eb-934d-936b1025c1cb.png)

## created a todo_list on PHP
## tested the connection and all worked as expected
![image](https://user-images.githubusercontent.com/36407447/115097143-5e03a880-9f20-11eb-9f00-39ad07790c3f.png)
