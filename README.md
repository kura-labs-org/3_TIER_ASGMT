<h1 align=center>Three Tier Application</h1>

## Goal:
The goal of this project is to create a [three-tier](https://docs.aws.amazon.com/whitepapers/latest/serverless-multi-tier-architectures-api-gateway-lambda/three-tier-architecture-overview.html) architecture application in an AWS VPC (Virtual Private Cloud) using cloudFormation. <br>

In the three-tier architecture, we have:
* **Data layer:** is the database where the data is stored.
* **Application layer:** This application layer is a [phpMyAdmin](https://www.phpmyadmin.net/) appication: it will manage the MySQL database hosted on AWS. 
* **Presentation layer:** Here, we have an NGINX application that act as a [reverse proxy](https://www.nginx.com/resources/glossary/reverse-proxy-server/) which helps the client access the backend.

* We will use **[bastion](https://aws.amazon.com/quickstart/architecture/linux-bastion/)** hosts to provide a secure access to the instances located in the private and public subnets of the VPC.

### NOTE: DELETE ALL THE RESOURCES AFTER YOU ARE DONE IN ORDER TO AVOID AN AWS BILL!!!

## Architecture:

![](images/tier3-0.PNG)

## Software Tools Used:
In this project, we used AWS services listed below.
* EC2
* Security Groups
* Internet Gateway
* Load Balancer
* Nat Gateway
* MySQL Database

## Precedure:

### Task 1: We will set up the resources using the  **[base.yaml](https://github.com/ibrahima1289/3_TIER_ASGMT/blob/main/base.yaml)** file.

1. Create a VPC with a public subnet and private subnet using cloudFormation; here is the [yaml](https://docs.aws.amazon.com/codebuild/latest/userguide/cloudformation-vpc-template.html) file.<br>
Create **yaml** file named **[base.yaml](https://github.com/ibrahima1289/3_TIER_ASGMT/blob/main/base.yaml)**; copy the content of the [yaml](https://docs.aws.amazon.com/codebuild/latest/userguide/cloudformation-vpc-template.html) file and paste/modify it into the **[base.yaml](https://github.com/ibrahima1289/3_TIER_ASGMT/blob/main/base.yaml)** file.<br>

  * The **base.yaml** file template will deploy:
    * a VPC, with a pair of public and private subnets spread across two Availability Zones 
    * an internet gateway, with a default route on the public subnets
    * a pair of NAT gateways (one in each AZ), and default routes for them in the private subnets
    
2. Now, save the **base.yaml** file locally.
3. Create a stack on cloudFormation.
```
CloudFormation -> Stacks -> Create stack
```
![](images/tier3-1.PNG)

4. Upload the **base.yaml** file.

![](images/tier3-2.PNG)

5. In the next page, give the stack a name

![](images/tier3-3.PNG)

6. We can see the parameters are the same as in the **base.yaml** file.
7. Select the **key pair** that will allow you to **ssh** into the differents **EC2**.

![](images/tier3-4.PNG)

  After hitting **next**, wait for a couple minutes in order for the stack to finish creating the resources.
  
![](images/tier3-5.PNG)

Once the stack is created, proceed to the next task.

![](images/tier3-6.PNG)

8. All the resources: phpmyadmin EC2 and Web proxy for caching will be created by the template base.yaml, and it’s secure.
9. After creating all the resources, **ssh** into the three **EC2** instances:
  * First, **ssh** into the **Bastion** EC2:
  
  * Follow the screenshots in order to find the command used to ssh.
  
   ![](images/tier3-7.PNG)
   
   ![](images/tier3-8.PNG)
   
   * Copy the command above and paste it into the command line interface.

   ![](images/tier3-9.PNG)
    
   * Now, run the commands ```sudo apt-get update -y``` and ```sudo apt-get upgrade -y```. This will update and upgrade the dependencies needed. And the ```-y``` will take ```yes``` as a default.
   
   ![](images/tier3-11.PNG)
   
   * Don't forget to add the ```key_pair_name.pem``` into the **Bastion** host before you ssh to the other instances from it. Also, make the ```key_pair_name.pem``` not available to the public by running the command ```sudo chmod 400 key_pair_name.pem```.
   
   ![](images/tier3-12.PNG)
   
   * Now, from the **Bastion EC2**, **ssh** into the **NGINX EC2** instances to make sure it works. Use the same methods as for the first EC2.
   * Once the ```update``` and ```upgrade``` is done, **exit** from the **NGINX** and **ssh** into the **phpMyAdmin** from the **Bastion** host, and, ```update``` and ```upgrade``` as well.
   
### Task 2: We will create a load balancer.

1. Create a target goup: a [target group](https://aws.amazon.com/blogs/aws/new-application-load-balancer-simplifies-deployment-with-weighted-target-groups/) is used to tell the load balancer where to direct traffic.

First, go to ```EC2 > Target groups > Create target groups```.

![](images/tier3-13.PNG)

Then, choose the **VPC** and port **80**. with **HTTP/HTTP1** protocols

![](images/tier3-14.PNG)

Register the **NginxProxy** to the target group and click on ```include as pending bellow``` with **port 80**.

![](images/tier3-15.PNG)

Now, create the target group.

![](images/tier3-16.PNG)

2. Create an AWS load balancer to connect to the reverse proxy.
* Go to AWS Loadbalancers and follow the instructions in the screenshots below.

![](images/tier3-17.PNG)

![](images/tier3-18.PNG)

![](images/tier3-19.PNG)

* Configure the **VPC** to the **Security Group** and select the **target group** created previously.

![](images/tier3-20.PNG)


### Task 3: We will create a database on AWS.

* Create a MySQL database on AWS by first creating a subnet group

a. Create a **subnet group** first.

![](images/tier3-22.PNG)

![](images/tier3-23.PNG)

![](images/tier3-24.PNG)


b. Now, create the database.

```Choose Free tier -> DB instance identifier -> Master username -> Master password -> Virtual private cloud (VPC) -> Select Subnet group -> VPC security group -> Choose existing (ThreeTierDB in this case) -> Availability Zone -> Create Database```

* Or, you can follow the screenshots below.

![](images/tier3-21.PNG)

![](images/tier3-25.PNG)

![](images/tier3-26.PNG)

![](images/tier3-27.PNG)

![](images/tier3-28.PNG)

### Task 4: We will set up the phpMyAdmin (application layer that interact with the database) to connect to the MySQL database we created on AWS.

1. Follow the istruction below after you **ssh** into the **phpMyAdmin** EC2 from the **Bastion** host:

- Update and upgrade: ```sudo apt-get update && sudo apt-get upgrade -y```
- Install apache2 by running the command: ```sudo apt-get install apache2 -y```.

![](images/tier3-29.PNG)

- Install the modules (mysql, php, apache) so that PHP can connect with mysql and apache: 

```sudo apt install php libapache2-mod-php php-mysql```

- Let's check php is working: 

Create a **test.php** by running this command ```touch /var/www/html/test.php``` and copy and paste the following  ```<?php phpinfo();``` into the **test.php** file.

- Install mysql: ```sudo apt install mysql-server -y```
- Run ```sudo mysql_secure_installation```

```Click Y -> 1```, enter the password, then enter ```Y```

- For the four following prompts, hit the ```Enter``` key for default.

- Log into mysql with to test the database: ```sudo mysql```

 ![](images/tier3-30.PNG)
 
- Run ```show databases;```

![](images/tier3-31.PNG)

-Exit from mysql database.

![](images/tier3-32.PNG)

- Install these packages (mbstring, json): ```sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl```

You will be prompted to this below, select **apache2** then click ```Enter``` for **Ok**.

![](images/tier3-33.PNG)

Select **yes** for the next prompt.

![](images/tier3-34.PNG)

Enter the mysql application password for phpMyAdmin.

![](images/tier3-35.PNG)

![](images/tier3-36.PNG)

Select **ok** to continue.

![](images/tier3-37.PNG)

- Abort the installation:

![](images/tier3-38.PNG)

- Log into the mysql database again: ```sudo mysql```
- Run this command to see that the root is empty: ```SELECT user,authentication_string,plugin,host FROM mysql.user;```

![](images/tier3-39.PNG)

- Run ```UNINSTALL COMPONENT "file://component_validate_password";```

![](images/tier3-40.PNG)

- Run ```INSTALL COMPONENT "file://component_validate_password";```

![](images/tier3-41.PNG)

- Exit from the mysql database and run ```sudo phpenmod mbstring``` in the **phpMyAdmin** shell.
- Log into mysql again: ```sudo mysql```

- Run ```ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';``` in the mysql shell.
- Then run again ```SELECT user,authentication_string,plugin,host FROM mysql.user;```

![](images/tier3-42.PNG)

Now we can see that the root has a value.

- Exit from the mysql shell: ```exit```

- Run ```cd /etc/php/7.4/apache2``` in the **phpMyAdmin** shell and edit the ```php.ini``` file by uncommenting line 895.

For this, run ```sudo nano php.ini``` after you run the command ```cd /etc/php/7.4/apache2```. Then click ```Alt + G```, this will prompt you to write the line number you want to edit. Type ```895```, and remove the semi-colon as it appears in the picture below.

![](images/tier3-43.PNG)

- ```cd /etc/apache2``` and edit the file **apache2.conf**. Add: ```include /etc/phpmyadmin/apache.conf``` at the end of the file.

![](images/tier3-44.PNG)

- Run ```sudo systemctl restart apache2```
- Connect the **AWS MySql** database to the **phpMyAdmin** EC2.<br.
Run ```sudo nano /etc/phpmyadmin/config.inc.php``` and paste the below content in the ```config.inc.php``` file.<br>

```$i++;
$cfg['Servers'][$i]['host'] = '__FILL_IN_DETAILS__';
$cfg['Servers'][$i]['port'] = '3306';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['extension'] = 'mysql';
$cfg['Servers'][$i]['compress'] = FALSE;
$cfg['Servers'][$i]['auth_type'] = 'config';
$cfg['Servers'][$i]['user'] = '__FILL_IN_DETAILS__';
$cfg['Servers'][$i]['password'] = '__FILL_IN_DETAILS__';
```

![](images/tier3-45.PNG)

2. Set up the Nginx reverse proxy

- Exit the **phpMyAdmin** EC2 by running: ```exit```

- **ssh** into the **Nginx** EC2 (reverse-proxy from the **Bastion** host
- Update and upgrade: ```sudo apt-get update && sudo apt-get upgrade -y```
- Install Nginx: ```sudo apt-get install nginx -y```
- Change into the **Sites available** by running ```cd /etc/nginx/sites-available/```. The **conf** files direct the proxy to the resources needed.
- Unlink the default **sites-enabled** file: ```sudo unlink /etc/nginx/sites-enabled/default```
- Unlink the **reverse-proxy.conf**: ```sudo unlink /etc/nginx/sites-enabled/reverse-proxy.conf```
- Create a configuration file for the Nginx: ```sudo nano reverse-proxy.conf```
- Copy and paste the script below in the reverse-proxy configuration file
```
server {
listen 80;
location / {
proxy_pass http://paste_phpMyAdmin_private_IP_here;
}
}
```

To save the file and exit, click ```Ctrl + X```, the click ```y``` for yes and hit ```Enter```.

- We can check the content od the directory **sites-enabled** by running: ```ls /etc/nginx/sites-enabled/```
- Let's link the **reverse-proxy** to **sites-enabled** in order for apache to access the Nginx: ```sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf```.
- Restart Nginx: ```sudo systemctl restart nginx```.

### Task 5: We will now access database using the reverse-proxy and the phpMyAdmin.

1. Go to ```AWS -> Load Balancers``` and copy the **DNS name**.

![](images/tier3-46.PNG)

2. Paste it on the url domain in a new browser.

![](images/tier3-47.PNG)

3. To access the database, type **http://DNS_name_url/phpmyadmin/index.php**. And put in the credentials to log in.

![](images/tier3-48.PNG)

![](images/tier3-49.PNG)

### Task 6: DELETE ALL TO AVOID A BILLING CHARGES FROM AWS.

For this, delete the stack and the database.

![](images/tier3-50.PNG)

## Sources visited:

1. https://jennapederson.com/blog/2021/6/21/provisioning-an-ec2-instance-with-cloudformation-part-1/
2. https://rdspg-monitoring.workshop.aws/en/prep/env/own-account.html
3. https://dev.classmethod.jp/articles/cloudformation-template-for-creating-ec2-with-load-balancer/
4. https://medium.com/the-andela-way/designing-a-three-tier-architecture-in-aws-e5c24671f124
