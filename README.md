<h1>VPC Project In Production Environment</h1>

<h2>Description</h2>
In this project, we are going to create a VPC that can be used for servers in a production environment. To improve resiliency, we deploy the servers in two Availability Zones, by using an Auto Scaling group and an Application Load Balancer. For additional security, we deploy the servers in private subnets. The servers receive requests through the load balancer. The servers can connect to the internet by using a NAT gateway. To improve resiliency, we deploy the NAT gateway in both Availability Zones.
<br />

<h2>Resources And Links Required:</h2>

- <b>docs.aws.amazon.com</b> https://docs.aws.amazon.com/vpc/latest/userguide/vpc-example-private-subnets-nat.html

<h2>Program walk-through Overview:</h2>

The implementation will be in done in 4 Steps:

STEP 1:  Create and Configure the VPC, Configure the subnets, NAT gateways, VPC endpoints, and Enable DNS hostnames.

STEP 2:  Deploy the application using Amazon EC2 Auto Scaling.

STEP 3:  Test the configuration.

STEP 4: Clean up

Project Overview: <br/>

This is the high level overview of what we are going to setup in this Project Lab:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/Design.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
<p align="center">

This diagram provides an overview of the resources included in this project. The VPC has public subnets and private subnets in two Availability Zones. Each public subnet contains a NAT gateway and a load balancer node. The servers run in the private subnets, are launched and terminated by using an Auto Scaling group, and receive traffic from the load balancer. The servers can connect to the internet by using the NAT gateway. The servers can connect to Amazon S3 by using a gateway VPC endpoint.

<h2>Program  Implementation:</h2>
 
STEP 1:  Create and Configure the VPC, Configure the subnets, NAT gateways, VPC endpoints, and Enable DNS hostnames.

1.1 Create and Configure the VPC: <br/>

Use Elastic Cloud registration link here above to Sign up for a free trial account. Then log in to the Elastic Cloud console, Click on “Start your free trial.”
Click on the “Create Deployment” button and select “Elasticsearch” as the deployment type. Choose a region and deployment size that fits your needs and click on “Create Deployment.”
Wait for the configuration to complete. Once the deployment is ready, click “continue.”

1.1.1 Free Elastic account set up  and Deployment created: <br/>

<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
1.2 Configure the subnets: <br/>

Go to the links provided here above, download and install Oracle Virtual Box, then download Kali Linux VM and save in a folder on the computer. virtual box  will be used to setup and run a virtual machine, Kali Linux VM  will be installed on the virtual machine.

Once the installation is complete, log in to the Kali VM using the credentials “kali” for both the username and password.

<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

1.3 Configure NAT gateways, VPC endpoints, and Enable DNS hostnames: <br/>

Go to the links provided here above, download and install Oracle Virtual Box, then download Kali Linux VM and save in a folder on the computer. virtual box  will be used to setup and run a virtual machine, Kali Linux VM  will be installed on the virtual machine.

Once the installation is complete, log in to the Kali VM using the credentials “kali” for both the username and password.

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
