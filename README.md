# 3_TIER_ASGMT

# Task 1

# Task 1 Procedure: Create a Public and Private Subnet VPC
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


# Task 1 Procedure: Creating a phpmyadmin EC2

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


# Task 1 Procedure: Create a Web Proxy:

# Task 2 

# Task 2 Procedure: Making a AWS Load Balancer


# Task 3

# Task 3 Procedure: Create AWS MySQL Database
