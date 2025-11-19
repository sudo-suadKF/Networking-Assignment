# Networking Assignment 
## Running Nginx on EC2 instance and linking it to a domain from Route 53
### In this assignment I will show you how to purchase a domain from AWS Route 53 and then link it to an EC2 istance. 
### The instance will run nginx on port 80, but then I will also show how to enable HTTPS because of the security, nginx will be accessed on port 443.

### At the end I will also explain and show how to customise the front end interface of the nginx and also how to create an elastic IP adress.

### Step 1: Buy a domain in Route 53:
Navigate to AWS Rote 53 and press Get started. Choose the option Register a domain and press Get started.

<img src="buy_domain.png"></img>

Choose a domain that is available. Observe the price, there are some expensive and cheap domains, then proceed to checkout.

### Step 2: Launch an EC2 instance:
Navigate to EC2 and press Launch Instance. Choose Ubuntu as the application.

<img src="ec2instance_ubuntu.png"></img>

At the instance type section, choose t2.micro. 

<img src="ec2_instancetype.png"></img>

Under the key pair section, create a new key pair. Enter a name, choose RSA as type and .pem as private key file format.

<img src="ec2_keypair.png"></img>

In Network settings, configure the security group by creating a new security group and check all the boxes, allow SSH from My IP and allow HTTP and HTTPS from internet.

<img src="ec2_networksettings.png"></img>

Now you have configured your instance, launch it!

### Step 3: SSH in to the EC2 instance:


