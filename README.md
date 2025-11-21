## Networking Assignment  
### Deploying Nginx on an EC2 Instance & Connecting It to a Route 53 Domain    
This project walks through the full process of purchasing a domain in AWS Route 53, launching an EC2 instance running **Nginx**, assigning an **Elastic IP**, and linking everything together using DNS A-records.  
You’ll also learn how to customize the Nginx frontend and also how to enable HTTPS on your domain.

### Step 1: Purchase a Domain in Route 53
- Navigate to **AWS Route 53** -> click **Get started**
- Select **Register a domain**
- Search for an available domain and complete checkout

<img src="images/buy_domain.png"></img>

### Step 2: Launch an EC2 Instance:
- Go to **EC2** -> click **Launch Instance**
- Choose **Ubuntu**

    <img src="images/ec2instance_ubuntu.png"></img>

- Select **t2.micro** instance type

    <img src="images/ec2_instancetype.png"></img>

- Create a new key pair (.pem, RSA)

    <img src="images/ec2_keypair.png"></img>

- Configure security group:
    - Allow SSH -> My IP
    - Allow HTTPS
    - Allow HTTP

    <img src="images/ec2_networksettings.png"></img>

Launch the instance.

### Step 3: SSH Into the EC2 Instance:
- Select your instance -> click **Connect**
- Open your terminal and navigate to the directory with your .pem file
- Secure the key:

    `chmod 400 <name_of_pem_file>.pem`

- Connect via SSH:

    `ssh -i <name_of_pem_file>.pem ubuntu@ec2-18-175-59-43.eu-west-2.compute.amazonaws.com`

    <img src="images/ec2_ssh.png">

### Step 4: Create & Assign an Elastic IP:
- Go to **Elastic IPs** -> **Allocate Elastic IP**
- Select your new Elastic IP -> **Actions** -> **Associate Elastic IP**
- Attach it to your running instance

 <img src="images/ec2_elasticIP.png"></img> <img src="images/ec2_assign_elasticIP.png"></img>

Why **Elastic IP?**  
Because EC2 public IPs change every time an instance is stopped.
Elastic IP ensures your address stays constant.

### Step 5: Link Your Domain (Route 53 -> EC2)
- Go to **Hosted Zones** -> **Create Hosted Zone**
    - Domain: **your purchased domain**
    - Type: **Public hosted zone**
- Click **Create record**
- Choose **A – IPv4 address**
- Enter your Elastic IP

<img src="images/route53_config_hostedzone.png"></img> <img src="images/route53_record.png"></img>

**What is an A Record?**  

It maps a **domain name** -> **IPv4 address**.  
This allows users to enter **yourdomain.com* instead of an IP.

DNS workflow (simplified):
- Browser asks DNS resolver
- If not cached → resolver asks Root Server
- Root Server → sends TLD server address
- TLD server → sends authoritative nameserver
- Nameserver → returns the correct IP
- Resolver → sends IP back to browser

### Step 6: Install & Run Nginx
- Update packages and install Nginx:

    `sudo apt update`  
    `sudo apt install nginx`

- Start Nginx:

    `sudo service nginx start`

Check status:

    `sudo service nginx status`

<img src="images/nginx_status.png"></img>

Now visit your domain or IP, the default Nginx page should appear.

<img src="images/nginx_webpage.png"></img>
  
### Step 7: Customize the Nginx Webpage
- Navigate to the default HTML directory:

    `cd /var/www/html`

- Open the HTML file:

    `sudo vim index.nginx-debian.html`

    Edit anything you want, then save.
- Test & reload Nginx:

    `sudo nginx -t`  
    `sudo systemctl reload nginx`

<img src="images/edit_nginx.png"></img> <img src="images/final_domain.png"></img>

### Step 8: Enable HTTPS on your domain
- Navigate to the the following directory:

    `cd /etc/nginx/sites-available`

- Enter the **default** file with text editor:

    `sudo vim default`

- Put your A record domain name next to the uncommented **server_name** at the middle of the file and save it. It should look like this:

    `server_name <your A reord domain>;`

    <img src="images/https_edit_default.png"></img>

- Test & reload Nginx:

    `sudo nginx -t`  
    `sudo systemctl reload nginx`

- Download certbort for nginx's HTTPS certification and type **yes** to continue downloading:

    ` sudo apt install certbot python3-certbot-nginx`

- Enable HTTPS certification for nginx to your A record domain by following:

    ` sudo certbot --nginx -d <your A record domain>`

    Enter your **email** and **Y** to the upcoming questions when you run the command.
    
    <img src="images/https_all_comm.png"></img> <img src="images/https_success.png"></img>


### Final Result
A fully deployed, fully customised Nginx server running on an EC2 instance, connected to your own Route 53 domain, with a stable Elastic IP and with HTTPS.

<img src="images/https_finally.png"></img>









