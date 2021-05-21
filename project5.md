# Implemention of Client-Server Architecture using MySQL Database Management System

### The goal of this project is to be able to connect from the instance named ``mysql client`` to the instance named ``mysql server`` 

## STEP 1
Launch and name 2 Linux-based EC2 instances in AWS as shown in the image below:
![image](https://user-images.githubusercontent.com/36407447/119158498-2cfe2280-ba4e-11eb-87c9-8475beddf45a.png)

## STEP 2
- Install MySQL server software on the EC2 instance named ``mysql server``
- Install MySQL client software on the EC2 instance named ``mysql client``

## STEP 3
Open port 3306 on ``mysql server`` as this is needed to communicate with the ``mysql client``
![image](https://user-images.githubusercontent.com/36407447/119161455-405ebd00-ba51-11eb-814e-0048eaa79a01.png)
For security concerns, allow only the IP address of the client to communicate with the server

## STEP 4
- Configure MySQL sever to allow connections from remote hosts. To achieve this, edit the config file as shown in the image below:
- Edit the config file using this command: ``sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf``
- Replace 127.0.0.1 to 0.0.0.0

![image](https://user-images.githubusercontent.com/36407447/119163371-42298000-ba53-11eb-8566-1facf01fa4e3.png)

## STEP 5
- On the ``mysql server``, create a user with the following command: ``CREATE USER 'newuser'@'%' IDENTIFIED WITH mysql_native_password BY 'passwor>';``
- Replace ``newuser`` and ``password`` with your desired username and password and continue with the commands below
- ``CREATE DATABASE test_db;``
- ``GRANT ALL ON test_db.* TO 'username'@'%' WITH GRANT OPTION;``
- ``FLUSH PRIVILEGES;``

## STEP 6
Connect from ``mysql client`` to ``mysql server`` by using the mysql utility as shown below:

``sudo mysql -u <username> -h <Private IP address of mysql server> -p``

![image](https://user-images.githubusercontent.com/36407447/119164206-16f36080-ba54-11eb-9947-6b364b132262.png)
