<h1>VPC Project In Production Environment</h1>

<h2>Description</h2>
In this project, we are going to create a VPC that can be used for servers in a production environment. To improve resiliency, we deploy the servers in two Availability Zones, by using an Auto Scaling group and an Application Load Balancer. For additional security, we deploy the servers in private subnets. The servers receive requests through the load balancer. The servers can connect to the internet by using a NAT gateway. To improve resiliency, we deploy the NAT gateway in both Availability Zones.
<br />

<h2>Resources And Links Required:</h2>

- <b>docs.aws.amazon.com</b> https://docs.aws.amazon.com/vpc/latest/userguide/vpc-example-private-subnets-nat.html

<h2>Project walk-through Steps:</h2>

The implementation will be in done in 4 Steps:

STEP 1:  Create and Configure the VPC, Configure the subnets, NAT gateways.

STEP 2:  Deploy the application using Amazon EC2 Auto Scaling.

STEP 3:  Test the configuration.

STEP 4: Clean up

<h2>Project high level overview :</h2>

This is the high level overview of what we are going to setup in this Project Lab:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/Design.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
<p align="center">

This diagram provides an overview of the resources included in this project. The VPC has public subnets and private subnets in two Availability Zones. Each public subnet contains a NAT gateway and a load balancer node. The servers run in the private subnets, are launched and terminated by using an Auto Scaling group, and receive traffic from the load balancer. The servers can connect to the internet by using the NAT gateway. The servers can connect to Amazon S3 by using a gateway VPC endpoint.

<h2>Project  Implementation:</h2>
 
STEP 1:  Create and Configure the VPC, Configure the subnets, NAT gateways, VPC endpoints, and Enable DNS hostnames.

1.1 Create and Configure the VPC: <br/>

By using the Amazon VPC console, in VPC settings we will select VPC and More option to create the VPC , It will automatically  create a route table for the public subnets with local routes and routes to the internet gateway. It will also create a route table for the private subnets with local routes, and routes to the NAT gateway, egress-only internet gateway. See on the diagram below: 

1.1.1 VPC creation with Subnets, Route Tables and Network Connections: <br/>

<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
1.2 VPC created with its resources, public and private subnets, route tables and internet gateway: <br/>

<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

1.3 Configure NAT gateways, VPC endpoints, and Enable DNS hostnames: <br/>

Go to the links provided here above, download and install Oracle Virtual Box, then download Kali Linux VM and save in a folder on the computer. virtual box  will be used to setup and run a virtual machine, Kali Linux VM  will be installed on the virtual machine.

Once the installation is complete, log in to the Kali VM using the credentials “kali” for both the username and password.

<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

STEP 2:  Deploy the application using Amazon EC2 Auto Scaling.

Auto sclaiing group required a launch template for its creation. Let create a launch template with security group where we will open ssh port and 8000( for the python application), accessible from anywhere.

 2.1 Creation of Create launch template: <br/>

 <img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

 2.2 Creation of AWS auto scalling: <br/>

 From the EC2 console, go to auto scalling, select the lauch template created here above, select the VPC created above and the privates subnets
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.2.1 VPC created with its resources, public and private subnets, route tables and internet gateway: <br/>

The auto scaling group created one EC2 in each availabity zone. Let notice that there is no IP address assigned to the instances in those private subnet.
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3 Bastion Host creation for application deployment into the EC2 instances in the private subnets: <br/>

since there is no IP address assigned to the EC2s in the private subnet, we need to create a bastion host from which we can access those intances.
From the EC2 console, let select launch instance, create a bastion host in the same VPC created above and enable security group ssh access from any where.
we will make make to enable auto assign public IP address
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3.1 Bastion Host connetion and EC2 access to deploy the application: <br/>

From our local computer, navigate to the folder that contains the key pair, change the chmod 400 "aws_login.pem" to make sure it will publicly viewable, 
Then the following command : ssh -i "aws_login.pem" ubuntu@ec2-3-82-148-3.compute-1.amazonaws.com  to access to the bastion host: 
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3.2 Bastion Host key pair transfer and connection to the private servers: <br/>

From our local computer, navigate to the folder that contains the key pair, change the permision on the key pair running the command: chmod 400 "aws_login.pem" to make sure it will publicly viewable, 
Then run the following command : ssh -i "aws_login.pem" ubuntu@ec2-3-82-148-3.compute-1.amazonaws.com  to access to the bastion host: 
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

To securely copy the key pair from our local computer to the bastion host, we need to run the following commad:
scp -i aws_login.pem remote-dev.pem ubuntu@ec2-34-227-31-201.compute-1.amazonaws.com:/home/ubuntu
once access to the bastion host, change the permision on the key pair running this commad: chmod 400 "remote-dev.pem", then using the private IP address, 
run this command: ssh -i "remote-dev.pem" ubuntu@10.0.137.165 to access the first EC2 instance created by the auto scaling froup:

Successful connected to one EC2 instance from the bastion Host
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.4 Basic Python deplyoment on the EC2 instance : <br/>
At this stage, we have successful accessed the first EC2 instance, now let create a simple html file and install a basic python application
using the command vim index.html, creaqte a basic index page, save it and run the application on port 8000 (python3 -m http.server 8000)
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.5 Creation of the load balancing and the target group : <br/>
Now we are going to create a load balancer in the public subnet, and attach instances in the privates subnets as target group:
From the EC2 dashboard, let scroll, select load balancers and click on create load balancers, choose the ALB (Application Load Balancer)
Load balancer should always be in the public subnet, that mean internet facing and connected the internet gateway

Load balancer should be is internet facing :
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Load balancer should be in the public subnet in a specific VPC:
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

From the "Create Application Load Balancer" scroll down to "Listeners and routing" section, select create Target Group. 

Target Group creation:
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Target Group creation(select include as pending while registring the target instances):
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Now, going back to the "Create Application Load Balancer" dashboard, scroll down to "Listeners and routing" section and refresh the target group Tab, then select the target group we just created.
and create the load balancer. 

Load balancer will from provisioning state:
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Load balancer will be ready when in active state:
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Mofify Load balancer listerner by opening port 80 in the security group to be able to access the load balancer from the internet:
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Now we can access the application deployed in the first private subnet from anywhere on the internet:
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

After installer the application on the instance in the second private subnet, we can see how the load balancer is balancing alternatively 
the traffic between the servers in both private subnets:

Load balancer deploying the application in the first private subnet 1
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Load balancer deploying the application in the second private subnet 2
<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />


CONCLUSION: <br/>

In this activity, we have set up a home lab using Elastic SIEM and a Kali VM. We forwarded data from the Kali VM to the SIEM using the Elastic Beats agent, generated security events on the Kali VM using Nmap, and queried and analyzed the logs in the SIEM using the Elastic web interface. We also created a dashboard to visualize security events and then created an alert to detect security events.


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
