# Configuring-and-Accessing-a-Linux-Virtual-Machine-on-AWS-EC2

https://youtu.be/crDwTtb47oU


Configuring and accessing a Linux Virtual Machine (VM) on AWS EC2 using a Linux operating system involves several steps. Below is a detailed guide to walk you through the process from creating an EC2 instance to accessing it via SSH.

1. Create an AWS EC2 Instance

Before you can access a Linux VM, you need to create an EC2 instance. Here's how you do it:

Step 1: Log into AWS Management Console

Go to AWS Management Console
.

Sign in with your credentials or create a new account if you don't have one.

Step 2: Launch a New EC2 Instance

Navigate to EC2 Dashboard:

On the console's main page, search for and click on EC2 to open the EC2 dashboard.

Launch Instance:

Click on the Launch Instance button to create a new virtual machine.

Choose an Amazon Machine Image (AMI):

Select a Linux-based AMI. Popular choices include:

Amazon Linux 2

Ubuntu (if you want to use Ubuntu)

Red Hat, etc.

For instance, choose Amazon Linux 2 for this guide.

Select an Instance Type:

Choose the instance type based on your needs. The free tier eligible instance is t2.micro, which is free for the first 750 hours per month.

Configure Instance Details:

You can leave the default settings unless you want to modify specifics like network settings or IAM roles.

Ensure that Auto-assign Public IP is enabled to allow SSH access via a public IP.

Add Storage:

The default storage (8 GB) is usually fine for basic usage. You can increase the size if needed.

Add Tags (Optional):

You can tag the instance for easy identification later. For example, Name: MyLinuxVM.

Configure Security Group:

Create a new security group that will allow access via SSH.

Add a rule for SSH (Port 22):

Source: My IP or Anywhere (0.0.0.0/0), depending on your security requirements.

This allows SSH access from your machine.

Review and Launch:

Review your settings and click Launch.

You will be prompted to select a Key Pair for SSH access.

If you don't have one, select Create a new key pair.

Download the private key file (e.g., my-key.pem). Do not lose this file, as it’s required to access the instance.

Launch Instance:

Click Launch Instances to finish the process.

Wait a few minutes for the instance to start.

2. Access the Linux VM via SSH

After the EC2 instance is running, you need to access it via SSH from your local Linux machine. Follow the steps below to connect:

Step 1: Find the Public IP Address of Your EC2 Instance

In the EC2 Dashboard, go to Instances.

Find your instance and check its Public IPv4 address. It will look like 54.x.x.x.

Step 2: Set Permissions for the SSH Key

Change the permissions of your .pem file to ensure it’s secure:

chmod 400 my-key.pem

Step 3: SSH into the Instance

In your local terminal, run the SSH command to access the instance. Replace my-key.pem with your actual key file, and ec2-user with the correct default user for your AMI:

For Amazon Linux 2, the default user is ec2-user.

For Ubuntu, the default user is ubuntu.

For Red Hat, it’s ec2-user.

ssh -i /path/to/my-key.pem ec2-user@<public-ip-address>


Example:

ssh -i ~/Downloads/my-key.pem ec2-user@54.123.45.67


If this is your first time connecting, you'll be prompted to confirm the host's authenticity. Type yes to continue.

Once authenticated, you should be logged into your Linux EC2 instance and see the shell prompt.

Step 4: Troubleshooting Access Issues

Permission Denied: If you see a "Permission denied" message, ensure the key file permissions are correct (chmod 400 my-key.pem).

Timeouts: Make sure your security group is configured correctly and that your local IP address is allowed in the security group rule for SSH (port 22).

Check Instance State: If the instance is in a "stopped" or "terminated" state, you won’t be able to access it. Ensure the instance is running.

3. Optional: Configure Your Instance for Use

Once you're logged into the instance, you can start configuring your Linux VM:

Update System Packages

It’s always a good idea to update your Linux instance after connecting for the first time:

# For Amazon Linux 2
sudo yum update -y

# For Ubuntu
sudo apt-get update && sudo apt-get upgrade -y

Install Software

You can install necessary software like web servers (e.g., Apache, Nginx), databases, or programming languages:

# For Amazon Linux 2 (example: install Apache HTTP server)
sudo yum install httpd -y

# For Ubuntu (example: install Apache HTTP server)
sudo apt-get install apache2 -y

Configure Firewall (if necessary)

On EC2, the firewall is controlled by the security group, but you can also configure firewalls within the instance using iptables or firewalld if you need more specific control.

4. Connect to Your EC2 Instance from Other Linux Machines

If you want to SSH into your EC2 instance from another Linux machine, you can follow the same SSH command process but using the key file and public IP of the instance.

5. Managing and Terminating the EC2 Instance
Stopping the Instance

You can stop the EC2 instance via the AWS console, but note that you’ll continue to be charged for the EBS volume (storage) even when it’s stopped.

Terminating the Instance

To stop incurring charges, you can terminate the instance from the AWS console (EC2 dashboard > Instances > select your instance > Actions > Instance State > Terminate).
