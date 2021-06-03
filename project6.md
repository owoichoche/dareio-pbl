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
running lsblk again should give you similar to result to the screenshot  
![image](https://user-images.githubusercontent.com/36407447/120670292-2e850d00-c488-11eb-85e0-e03bb5d66dbb.png)

Next, run `sudo yum install lvm2`
then run
`sudo pvcreate /dev/xvdf1`
`sudo pvcreate /dev/xvdg1`
`sudo pvcreate /dev/xvdh1`

run `sudo pvs` to verify that the Physical volume has been created successfully

run `sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1` to add all Physical Volumes to a volume group.

run `sudo vgs` to verify that VG has been successfully created

create 2 logcal volumes as follows:
`sudo lvcreate -n apps-lv -L 14G webdata-vg`
`sudo lvcreate -n logs-lv -L 14G webdata-vg`
