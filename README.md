# 3_TIER_ASGMT
<h1 align=center>3 Tier assignment</h1>

<h2>Welcome to the 3 Tier assignment repo.</h2>  

- Make a pull request to submit your assignment: 


<h1>Documentation</h1>
  
<h2> Goal </h2>

The goal of this assignment was to manually create a 3-tiered network on AWS through clickOps operations to create and understand how to build a secure infrastructure based on previous learning sessions. For this learner, the goal will be utilizing SG as the foundation to creating safeguards in egress and ingress of traffic, the use of CA certs for the purpose of creating secure encrypted traffic to prevent snooping via AWS ACM and to increase availability of service through the use of availability using application load balancing services provided on AWS.

<h2> Things we need to do </h2>

- Create a VPC that spans over 2 availability zones with public and private subnet. This means a minimum of 4 subnets needed. Since this is a small-scale project with limited intentions for additional devices on the network, this will use a 255.255.255.0 VPC, subnet /24 and a /26 subnet, which means a total division of 62 IPs usable per subnet.
- Creating of 2 EC2s. The purpose of EC_1 will be used as a reverse proxy and caching service and will be placed in public subnet. This is done to ensure that exposure of traffic sent to the next program will be masked, so that means the incoming and outgoing traffic will look like it is coming from the reverse proxy instead of the actual server that hosts the application. The benefits of this is a security standpoint and performance. When traffic hits the reverse proxy, it can handle the load of the traffic and redirect it to a server or multiple servers to ensure that a proper load is sufficient to handle multiple incoming and outgoing requests. The caching service on this server will also help as any preexisting data that was grabbed by the EC_1 from the application will be kept in temporary storage to ensure that there is less of a need to create additional requests for data that it will already have ready and available.
- For the EC_2. This will serve as a database management application and be hosted on a private subnet. The underlying application that is used is an apache2 web server, which will run phpMyAdmin. This application is used to manage MySQL databases both locally and remotely. The application will be contacting a remote server on AWS that hosts a MySQL database.
- Creation of MySQL database. This database will be a standard MySQL database that can be hosted on multiple availability zones and in a private subnet. This ensure that the data is kept safe, will be read from the phpMyAdmin program and be replicated over another AZ to ensure failover and redundancy of data.
- Application Load Balancer: We will need to set up an Application Load Balancer with a target group that hits on the Nginx server (EC_1). This will ensure that the nginx server will have 2 availability zones for ensuring increased availability so we can reach our database.
- ACM- The ACM service by AWS can be used to create a CA cert that can be attached to the ALB. This is used to ensure the applications we use will have an SSL connection that is recognized by all users as certs by ACM are recognized by the authoritative name server. The benefits of using this an on an ALB is to reduce resource usage on the other existing servers as SSL itself can be a resource that taxes the system.
- Optional(Bastion) - Bastion can be used as a jump drive relay to allow all systems within a VPC are within a private subnet, ensuring further security.


While most of the steps for creation and process are explained within the [pdf](https://github.com/kawangwong/3_TIER_ASGMT/blob/main/3%20Tier%20assignment.pdf). What is not explained is the specific how to, security and the construct for the topology.

<h3>Specific How-to</h3>

For this case, some of the troubles I ran into as the system architect was how to set everything in the private subnet up, then flip them to ensure that it is up to date and usable by the end user.
- One solution to setting things up is the division of subnets and the public to private method. Besides the server, for the EC2, you are able unable to change an EC2 subnet once it is created on the subnet, however, you may be able to snapshot the image, the recreate it on a private subnet. Just be sure to undo the public IP setting and change SG group.
- You may also flip the subnet from public to private and do the same for a private to public to ensure balance.
- The last method is to use a Nat Gateway and elastic IP to do outgoing data requests from the private IP.

<h3> SSL </h3>
- There are various ways to ensure SSL here. First is through ACM
- Second is through Nginx and creating a self-signed CA.
- Last is on apache2, then creating a self-signed CA.

The benefits of ACM were stated above, but it is essentially reduction of resource used by EC2 and having a CA cert that is signed and recognized by an authoritative server.
