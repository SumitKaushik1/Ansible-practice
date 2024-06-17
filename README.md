# Ansible-practice
## Setting Up an EC2 Instance On AWS 
- Create an EC2 Instance:

- Choose Ubuntu Server 24.04 as the Amazon Machine Image (AMI).
- Select the t2.micro instance type (free tier eligible).
- Generate a .pem key file (e.g., portfolio1.pem) for SSH access.
- Launch the Instance:

- Review your settings and click on Launch Instance.
- Configure Security Group:

- Go to the Security Group tab and add the following inbound rules:
 - Type: HTTPS, Source Type: Custom, Source: 0.0.0.0/0, Description: For SSL certificate
   - Note: If you don't have an SSL certificate, avoid using port numbers associated with HTTPS (typically port 443). If HTTPS is enabled on a port and there's no SSL certificate, browsers will attempt to connect via HTTPS by default. If HTTPS is disabled, traffic defaults to HTTP instead. This is why it's important to configure ports accordingly based on your SSL certificate status to ensure traffic is routed correctly.
 - Type: HTTP, Source Type: Custom, Source: 0.0.0.0/0, Description: For Apache server
 - Type: Custom TCP, Source Type: Custom, Source: 0.0.0.0/0, Port Range: 9100, Description: For Node Exporter
 - Save the Security Group Rules.

- Connecting to the EC2 Instance
- Open Git Bash.
- Type the following command to connect to your EC2 instance via SSH:
  - eg ssh -i "port-folio1.pem" ubuntu@ec2-65-1-114-222.ap-south-1.compute.amazonaws.com

Above steps are followed to make to aws instances ansible-practice and target-server-ansible



# To establish a passwordless connection between machines using Ansible, follow these detailed steps:

## Set Up Ansible on the Ansible Practice Instance:

### Update the package list:

````sh
sudo apt update
````

### Install Ansible:

````sh
sudo apt install ansible
````

### Generate SSH Key Pair:

### Generate a new SSH key pair:

````sh
ssh-keygen
````
Press Enter or type 'Y' to accept the default location and settings.

### Identify the Public Key Location:

According to the tutorial, the public key is located at /home/ubuntu/.ssh/id_rsa.pub. However, the actual file location is:

`````sh
/root/.ssh/id_ed25519.pub
`````
Display the contents of the public key:

````sh
cat /root/.ssh/id_ed25519.pub
````
### Copy the Public Key:

The output should resemble the following:

`````sh
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHNctYoc+2YhOUMvfG13HzMMpJ1yXvdqwq86knUhnMGs root@ip-172-31-12-167
````````
 Copy this public key.
## Add the Public Key to the Target Server:

- Log into the target server.
 ### Firsty  generate a new SSH key pair:

````sh
ssh-keygen
````
  
- Open the authorized_keys file (create it if it doesn't exist):
`````sh
nano /root/.ssh/authorized_keys
`````
- Paste the copied public key from ansible instance into this file of target instance
- Save and exit the editor (Ctrl+S and Ctrl+X).
### Test the Connection:

- Return to the Ansible practice server.
- Since both instances are on the same AWS network, use the private IP address to connect (no need for a public IP address):
````sh
ssh <target_server_private_ip>
`````
Example:

````sh
ssh 172.31.35.205
````
### Verify Passwordless Connection:

Ensure that you can connect to the target server without being prompted for a password. This confirms that the SSH key authentication is correctly set up.
With this setup, Ansible can now connect to the target machine without requiring password authentication, enabling efficient and secure automation of tasks across your instances.
