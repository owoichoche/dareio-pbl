# Web Solution with Wordpress
In this lesson, we shall be building a web solution with Wordpress, leveraging on the 3-tier architecture which comprises of the following:
1. The Presentation Layer - This refers to the user interface and will be acessed by a browser on the Client Computer.
2. The Business or Application Layer - This is the backend program that implements the business logic. This will be represented with the Web Server. 
3. The Data Access or Management Layer - This is the layer of data storage and data access. Usually, a database Server is used for this purpose.

#### Note: We will be using RedHat Linux on AWS for this project 

## STEP 1 - Prepare a Web Server
1. Spin up an EC2 instance
2. Create 3 volumes in the same Availability zone and attach these volumes to the Server. Follow this link to add an EBS volume to the Server- https://www.youtube.com/watch?v=HPXnXkBzIHw
3. connect to the server and open the Linux terminal to begin configuration
4. run the `lsblk` command to get such result below. This command is used to inspect block devices attached to your server.
![image](https://user-images.githubusercontent.com/36407447/120668480-6a1ed780-c486-11eb-9e93-4c7b1c4745f1.png)
5. run `df -h` to see all mounts and free space on the server
6. run `gdisk` to creat a single partition on each of the 3 disks as follows:
- `sudo gdisk /dev/xvdf`
- `sudo gdisk /dev/xvdg`
- `sudo gdisk /dev/xvdh`
running `lsblk` again should give you similar to result to the screenshot below
![image](https://user-images.githubusercontent.com/36407447/120670292-2e850d00-c488-11eb-85e0-e03bb5d66dbb.png)

Next, run the following one after the other
- `sudo yum install lvm2`
- `sudo pvcreate /dev/xvdf1`
- `sudo pvcreate /dev/xvdg1`
- `sudo pvcreate /dev/xvdh1`

run `sudo pvs` to verify that the Physical volume has been created successfully

run `sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1` to add all Physical volumes to a volume group.

run `sudo vgs` to verify that VG has been successfully created

create 2 logical volumes as follows:

`sudo lvcreate -n apps-lv -L 14G webdata-vg`

`sudo lvcreate -n logs-lv -L 14G webdata-vg`

To verify that the logical volume has been created, run `sudo lvs`

To view the entire setup run 

`sudo vgdisplay -v`

`sudo lsblk`

#### Format Logical volumes
`sudo mkfs -t ext4 /dev/webdata-vg/apps-lv`

`sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`

#### Create /var/www/html directory to store website files

`sudo mkdir -p /var/www/html`

#### Create /home/recovery/logs to store backup of log data

`sudo mkdir -p /home/recovery/logs`

#### Mount /var/www/html on `apps-lv` logical volume

`sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

#### Use `rsync` utility to backup all the files in the log directory

`sudo rsync -av /var/log/. /home/recovery/logs/`

#### Mount `/var/log` on `logs-lv` logical volume. Note that all existing data on `/var/log` will be deleted

`sudo mount /dev/webdata-vg/logs-lv /var/log`

##### Restore log files back into /var/log directory 

`sudo rsync -av /home/recovery/logs/log/. /var/log`

#### Update `/etc/fstab` file so that the mount configuration will persist after restarting the server

#### Edit the `/etc/fstab` file

`sudo vi /etc/fstab`

Update `/etc/fstab` in this format using your own UUID and rememeber to remove the leading and ending quotes.
![image](https://user-images.githubusercontent.com/36407447/120719329-fac6d900-c4c1-11eb-9c53-082ae065201c.png)

#### Test the configuration, reload the daemon and verify setup

`sudo mount -a`

`sudo systemctl daemon-reload`

`df -h`

## Step 2 - Prepare the Database Server

Launch an EC2 instance (RedHat) and set it up like the Server in Step 1.

Replace `apps-lv` with `db-lv`

mount it to `/db` directory instead of `/var/www/html/`

## Step 3 - Install Wordpress on Web Server

#### Update the repository

`sudo yum -y update`
![image](https://user-images.githubusercontent.com/36407447/120719693-9b1cfd80-c4c2-11eb-9798-7b6b8a91874b.png)

#### Install wget, Apache and it’s dependencies

`sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`

#### Start Apache

`sudo systemctl enable httpd`

`sudo systemctl start httpd`

#### Install PHP and its dependencies

`sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm`

`sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm`

`sudo yum module list php`

`sudo yum module reset php`

`sudo yum module enable php:remi-7.4`

`sudo yum install php php-opcache php-gd php-curl php-mysqlnd`

`sudo systemctl start php-fpm`

`sudo systemctl enable php-fpm`

`setsebool -P httpd_execmem 1`

#### Restart Apache
`sudo systemctl restart httpd`

#### Confirm Apache is successfully installed by entering the Pubic IP addres of the Web Server in the browser
![image](https://user-images.githubusercontent.com/36407447/120720567-35317580-c4c4-11eb-90ab-823fcf3f4760.png)

#### Download wordpress and copy wordpress to `var/www/html`

`mkdir wordpress`

`cd   wordpress`

`sudo wget http://wordpress.org/latest.tar.gz`

`sudo tar xzvf latest.tar.gz`

`sudo rm -rf latest.tar.gz`

`cp wordpress/wp-config-sample.php wordpress/wp-config.php`

`cp -R wordpress /var/www/html/`

#### Configure SELinux Policies

`sudo chown -R apache:apache /var/www/html/wordpress`

`sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R`

`sudo setsebool -P httpd_can_network_connect=1`

## Step 4 — Install MySQL on DB Server

`sudo yum update`

`sudo yum install mysql-server`

#### Verify that the service is running

`sudo systemctl status mysqld`

`sudo systemctl restart mysqld`

`sudo systemctl enable mysqld`

## Step 5 — Configure DB to work with WordPress

`sudo mysql`

`CREATE DATABASE wordpress;`

`CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';`

`GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';`
  
`FLUSH PRIVILEGES;`
  
`SHOW DATABASES;
  
`exit`
  
## Step 6 — Configure WordPress to connect to remote database

#### Install MySQL client

`sudo yum install mysql`
  
#### Test connection from Web Server to DB server by using mysql-client

`sudo mysql -h <DB-Server-Private-IP-address> -u wordpress -p 
![image](https://user-images.githubusercontent.com/36407447/120723122-036edd80-c4c9-11eb-901d-bfd3568cd4b3.png)

#### Access from the browser the link to the WordPress http://<Web-Server-Public-IP-Address>/wordpress/ or just the Public IP address of the Web Server
![image](https://user-images.githubusercontent.com/36407447/120723254-4cbf2d00-c4c9-11eb-8a8c-9bb3b70e4504.png)

![image](https://user-images.githubusercontent.com/36407447/120723292-5ea0d000-c4c9-11eb-8045-ae7c763e0300.png)

![image](https://user-images.githubusercontent.com/36407447/120723315-6a8c9200-c4c9-11eb-9791-6a360b6bf05a.png)

![image](https://user-images.githubusercontent.com/36407447/120723332-72e4cd00-c4c9-11eb-8d00-9a4d206c7ea4.png)

  
  
  
 
  
  









