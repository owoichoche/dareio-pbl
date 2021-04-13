# Project 1

## Prerequisites - Connected to EC2 instance

1. I spun an EC2 instance on AWS and installed Ubuntu Linux on it
1. downloaded and installed **Putty** on my Windows computer and connected via **SSH** to the EC2 instance.
2. ![image](https://user-images.githubusercontent.com/36407447/114519795-a7aa7580-9c38-11eb-9c8d-d4f3e25b2185.png)

## Installed Apache and Updated the firewall
1. Installed Apache using Ubuntu's package manager 'apt'
2. added a rule to EC2 to open inbound connection through port 80
3. ![image](https://user-images.githubusercontent.com/36407447/114521216-1e943e00-9c3a-11eb-8614-2d79fbd755cf.png)
4. verified that apache2 was running as a service on Ubuntu as you can see from a screenshot from my environment below.
5. ![image](https://user-images.githubusercontent.com/36407447/114520634-8eee8f80-9c39-11eb-8d4f-0075b145d743.png)

4. tested apache's http server requests by entering my server's public IP address into my browser and got the following result in return:
5. ![image](https://user-images.githubusercontent.com/36407447/114520676-97df6100-9c39-11eb-8676-4fa53d529876.png)

## Installed MySQL
1. installed MySQL using the ***apt*** install command
1. confirmed successful install by using the **mysl** command as captured in the screenshot below
2. ![image](https://user-images.githubusercontent.com/36407447/114520279-2dc6bc00-9c39-11eb-801f-ca0086d4a28b.png)

## Installed PHP
- installed all 3 packages by using the ***apt install php libapache2-mod-php php-mysql*** command
- confirmed the PHP version installed as shown below
- ![image](https://user-images.githubusercontent.com/36407447/114523008-cfe7a380-9c3b-11eb-9c86-642b7ddc1467.png)

## Created a virtual host for my website using Apache
1. created an index.html file and used the ***echo*** command to print same on screen
1. see the result of the **echo** below:
2. ![image](https://user-images.githubusercontent.com/36407447/114520863-c826ff80-9c39-11eb-9f00-aefda3a0c0d7.png)

## Enabled PHP on the website
- see the PHP page below
![image](https://user-images.githubusercontent.com/36407447/114520970-e260dd80-9c39-11eb-8a3e-13826710f0fa.png)
