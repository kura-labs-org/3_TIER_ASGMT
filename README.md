# 3_TIER_ASGMT
A 3-tier architecture in technical terms are three following technologies that are connected together: 
* Cilent or host computer
* Server
* Database
<br>

This documentation will explain one possible way how to make a 3-tier architecture on AWS by completing the following tasks:
* Make a public and private subnet
* Create a phpmyadmin EC2
* Develop a Web Proxy for caching
* Initiate an AWS Load Balancer that is connected to the reverse proxy
* Lastly, make a MySQL database on AWS 

# Task 1

## Task 1 Procedure: Create a Public and Private Subnet VPC
1. Use the link down below to make a VPC on Cloudformation: <br>
https://docs.aws.amazon.com/codebuild/latest/userguide/cloudformation-vpc-template.html

2.  Copy the Cloudformation VPC template from the link provided and paste it to a new document and name it with in the following yaml format: "".yaml <br>
<br>
"" - is the name you can name your yaml file <br>
In this demonstration, the yaml file is called vpc_temp.yaml <br>
<br>

Below is the image of the VPC template that will be used:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/VPC/1.png width="1000" />
     </h1>
</html> 

3. Go to Cloudformation and click "Create Stack" and choose "With new resources (standard)":

<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/VPC/2.png width="1000" />
     </h1>
</html> 


4. Choose "Template is ready", "Upload a template file" and click "Choose file" to upload your yaml file with the VPC template:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/VPC/3.png width="1000" />
     </h1>
</html> 

5. When configuring the stack, give it a stack name and leave all the parameters as default:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/VPC/4.png width="1000" />
     </h1>
</html> 


6. Create the stack:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/VPC/5.png width="1000" />
     </h1>
</html> 


## Task 1 Procedure: Creating a phpmyadmin EC2

1. Create an EC2 on AWS. In this tutorial, the Ubuntu 20.0.4 EC2 will be used. After creating it, log in into the EC2. <br>
<br>

2. Run the following command:
```
sudo apt update
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/1.png width="1000" />
     </h1>
</html> 

Proceeding sudo apt update, run:
```
sudo apt upgrade
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/2.png width="1000" />
     </h1>
</html> 

Running "sudo apt update" updates all the existing packages an EC2 could already download by giving it more options to download more packages that were recently made or give the EC2 the latest version of existing packages. Executing the command "sudo apt upgrade" offically updates the EC2's packages. <br>
<br>

3. In this EC2, apache2 will be installed by performing:
```
sudo apt install apache2
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/3.png width="1000" />
     </h1>
</html> 

4. Go to a web browser and paste the EC2's public address. The Apache2 Ubuntu Default Page should show up to display it successfully downloaded Apache:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/4.png width="1000" />
     </h1>
</html> 

5. More packages will now be installed onto the EC2:
```
sudo apt install php libapache2-mod-php php-mysql
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/5.png width="1000" />
     </h1>
</html> 

These packages are responsible for downloading php. <br>
<br>

6. Change directories to "/var/www/html". Changing to this directory can be done by typing this in the terminal: 
```
cd /var/www/html
```
Make sure the statement is well spaced as shown above otherwise it won't work
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/7.png width="1000" />
     </h1>
</html>

7. In the "/var/www/html", create a test.php file:
```
sudo nano test.php
```
This directory requires admin privileges therefore each command must start with sudo <br>
nano is a command to make a file and the words proceeding after nano is the name of the file and also its extension <br>
<br>

8. In the test.php file write the following shown below:
```
<?php phpinfo();
```
Save the file by pressing 'Ctrl + S' and then press 'Ctrl + X'  exit out of the test.php file after saving
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/8.png width="1000" />
     </h1>
</html> 
9. Go to the web browser and enter the EC2 public ip address followed by /test.php to see if php was successfully installed.
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/9.png width="1000" />
     </h1>
</html> 

10. The next package mysql-server will be installed:
```
sudo apt install mysql-server
```

11. After installing mysql server, run:
```
sudo mysql_secure_installation
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/11.png width="1000" />
     </h1>
</html>

12. 'mysql_secure_installation' then shows instructions to install a password
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/12.png width="1000" />
     </h1>
</html> 
13. Choose and enter a **strong** password and **remember the password as it will be needed** and skip: <br>
Remove anonymous users <br> 
Disallow root login remotely <br> 
Remove test database <br> 
Reload privilege tables <br>
<br>
You can just press Enter to skip these questions. <br>
<br>

14. Enter:
```
sudo mysql
```
Now the terminal will display a line where it enters into MySQL<br>
<br>
15. More packages are needed to be install phpmyadmin and to configure a password when always entering phpmyadmin:
```
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/15.png width="1000" />
     </h1>
</html>
16. Choose apache2, the red light shows what option you will choose. Press enter to confirm. 
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/16.png width="1000" />
     </h1>
</html> 
17. Choose yes:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/17.png width="1000" />
     </h1>
</html>
18. Enter the password:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/18.png width="1000" />
     </h1>
</html>

19. If an error occurred, run the commands down below to change the password:
```
sudo apt install phpmyadmin
sudo phpenmod mbstring
```

<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/21.png width="1000" />
     </h1>
</html> 
A successful run should show the following:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/22.png width="1000" />
     </h1>
</html> 
Where a password can be entered in the '' at the end of the statement: ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY ''; <br>
Do not dispose of the ''. Place the password in the ''. <br>
<br>
A password should contain: <br>
At least 1 lowercase letter <br>
At least 1 uppercase letter<br>
8 characters <br>
At least 1 number <br>
At least 1 special character (such as an !) <br>
<br>

20. Run the sudo mysql to see the new password:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/23.png width="1000" />
     </h1>
</html>

21. Go to this directory and make a file:
```
cd /etc/php/7.4/apache2
sudo nano php.ini
```
22. In the apache2 file, press Alt + G to go to line 895:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/24.png width="1000" />
     </h1>
</html> 
23. Remove the ; on line 895 as shown below in the picture:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/25.png width="1000" />
     </h1>
</html> 

24. Run:
```
sudo nano /etc/apache2/apache2.conf
```
25. On the bottom of the page, write:
```
include /etc/phpmyadmin/apache.conf
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/26.png width="1000" />
     </h1>
</html> 

26. Run:
```
sudo systemctl restart apache2
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/27.png width="1000" />
     </h1>
</html> 

27. Run:
```
sudo systemctl status apache2
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/28.png width="1000" />
     </h1>
</html> 

28. Go to the web browser to check for phpmyadmin. Enter in the web browser the EC2 public IP address followed by /phpmyadmin/
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/phpmyadmin/29.png width="1000" />
     </h1>
</html> 
You can login by entering 'root' for the username and the password you created before for password

## Task 1 Procedure: Create a Web Proxy

Create another EC2 and have the phpmyadmin EC2 running as well <br>
<br>
2. This EC2 will be referenced as webtier in this documentation <br>
<br>
3. Include ports ssh and http that can be accessed anywhere <br>
4. 
```

```

```

```
5. Nginx will now be installed on EC2 by the following command:
```
sudo apt install nginx
```

<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/6.png width="1000" />
     </h1>
</html> 

6. After downloading Nginx, the enter the "cd /etc/nginx/sites-available" with the following command:
```
cd /etc/nginx/sites-available
```
Then run
```
ls
```
There should be one file called "default"<br>
<br>
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/7.png width="1000" />
     </h1>
</html> 

7. Run the following command to unlink the default file to release the default settings Nginx was following:
```
sudo unlink /etc/nginx/sites-enabled/default
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/8.png width="1000" />
     </h1>
</html> 
By unlinking the default file, a new file can be made to replace the default file to configure a reverse proxy. <br>
<br>

8. Make a file to enable a with a .conf extension:
```
sudo nano reverse-proxy.conf
```
9. Paste the following code in the reverse-proxy document:
```
server {
    listen 80;
    location / {
        proxy_pass http://''
```
'' is private ip address of the ec2 with phpmyadmin and apache already installed <br>
Save with CTRL + S and exit with CTRL + X <br>
<br>

<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/9.png width="1000" />
     </h1>
</html> 

10. See what files are in this directory:
```
ls /etc/nginx/sites-enabled
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/10.png width="1000" />
     </h1>
</html> 

11. Run the command to enable the new .conf file:
```
sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/11.png width="1000" />
     </h1>
</html> 

12. Re-running the "ls /etc/nginx/sites-enabled" will now show "reverse-proxy.conf" in blue to show it is now enabled and the application is on port 80:
```
ls /etc/nginx/sites-enabled
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/12.png width="1000" />
     </h1>
</html>

13. Now reenter back into the home directory:
```
cd
```
Now the terminal is in the /etc/nginx directory. Run:
```
ls
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/13.png width="1000" />
     </h1>
</html> 
There are many files and there should be a file called nginx.conf. <br>
<br>

14. Edit the nginx.conf file with sudo permissions:
```
sudo nano nginx.conf
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/14.png width="1000" />
     </h1>
</html> 

15. Nginx should now be successfully installed. Check the status to see if it is installed.
```
systemctl status nginx
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/15.png width="1000" />
     </h1>
</html> 

16. Take the web tier EC2 public address and log in to Apache and phpmyadmin:      
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/16.png width="1000" />
     </h1>
</html> 
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/17.png width="1000" />
     </h1>
</html> 
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task1/webproxy/18.png width="1000" />
     </h1>
</html> 

# Task 2 

## Task 2 Procedure: Making a AWS Load Balancer
1. Go to the EC2 Page <br> 
<br>
2. On the left side of the screen, there is a menu. Written in large gray font is 'Load Balancing'. Click the subsection under it called 'Target Groups'.
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task2/1.png width="1000" />
     </h1>
</html> 
3. When customizing the load balancer, use all default settings and also give it a target group name <br>
<br>
4. Under Available instances, choose the reverse proxy ec2 and create the target group:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task2/2.png width="1000" />
     </h1>
</html> 

# Task 3

## Task 3 Procedure: Create AWS MySQL Database
1. Go to Amazon RDS <br>
<br>
2. Click "Create Database" <br>
<br>
3. Select mysql and standard create
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/1.png width="1000" />
     </h1>
</html> 

4. Under Settings remember the name of the DB instance and the master username 
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/2.png width="1000" />
     </h1>
</html>
Also make a password and confirm as it will be needed to access this database. <br>
<br>
   Under connectivity select a VPC. The default public VPC is used in this demonstration.
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/3.png width="1000" />
     </h1>
</html>

   Under templates select free tier
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/4.png width="1000" />
     </h1>
</html> 

5. Reenter the phpmyadmin EC2 and access the 'etc/phpmyadmin directory' by using this command:
```
cd /etc/phpmyadmin/
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/5.png width="1000" />
     </h1>
</html> 
Afterwards use the ls command to see the files:
```
ls
```
There should be a file called "config.inc.php". This file needs to be edited but it is in a root directory, therefore the command sudo nano is neccessary to access and edit the file:
```
sudo nano config.inc.php
```
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/6.png width="1000" />
     </h1>
</html> 
6. Use ALT + G to go to line 102. Below the }, paste the following code: <br>
<br>
$i++;                                                         <br>
$cfg['Servers'][$i]['host']          = '__FILL_IN_DETAILS__'; <br>
$cfg['Servers'][$i]['port']          = '3306'; <br>
$cfg['Servers'][$i]['socket']        = ''; <br>
$cfg['Servers'][$i]['connect_type']  = 'tcp'; <br>
$cfg['Servers'][$i]['extension']     = 'mysql'; <br>
$cfg['Servers'][$i]['compress']      = FALSE; <br>
$cfg['Servers'][$i]['auth_type']     = 'config'; <br>
$cfg['Servers'][$i]['user']          = '__FILL_IN_DETAILS__'; <br>
$cfg['Servers'][$i]['password']      = '__FILL_IN_DETAILS__' <br>
<br>
Make sure to change FILL in DETAILS lines <br>
<br>
host - endpoint url of data base which is found on AWS RDS page <br>
Click on Credential Details on AWS for user and password 

<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/7.png width="1000" />
     </h1>
</html> 
7. Access the database through the internet browser by copying the endpoint: <br>
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/8.png width="1000" />
     </h1>
</html>
There are two server choices to choose from:
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/9.png width="1000" />
     </h1>
</html>
<html>
     <h1>
        <img style="float: center;" src=/3tierpictures/task3/10.png width="1000" />
     </h1>
</html> 
