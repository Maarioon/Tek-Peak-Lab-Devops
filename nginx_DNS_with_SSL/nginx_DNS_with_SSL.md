# NGINX Web Server Setup on AWS EC2 Using Bash Script as User Data

This guide demonstrates how to create and launch an AWS EC2 instance with an NGINX web server using a Bash script provided as user data. Additionally, it covers how to secure the server with SSL and set up a custom domain name.

## Prerequisites

Before proceeding, ensure you have the following if you havn't done it from the previous nginx deployment:

- AWS Account
- AWS CLI configured with appropriate permissions
- SSH access to EC2 instances
- A registered domain name
- AWS Route 53 for managing DNS (optional)
- Basic knowledge of EC2, NGINX, and SSL

## Step 1: Create a Bash Script for NGINX Installation

The Bash script will automate the installation of NGINX and start the web server. This script will be added as EC2 user data.

```bash
#!/bin/bash
# Update package information
sudo apt update

# Install NGINX
sudo apt install -y nginx

# Start NGINX service
sudo systemctl start nginx

# Enable NGINX to start on boot
sudo systemctl enable nginx

```

## Step 2: Launch EC2 Instance with the User Data Script

1. Open the AWS Management Console and navigate to EC2.
2. Click **Launch Instance**.
3. Choose an Amazon Machine Image (AMI) (e.g., Ubuntu 20.04).
4. Select an instance type (e.g., t2.micro for the free tier).
5. In the **Configure Instance** section, expand **Advanced Details**.
6. In the **User data** field, copy and paste the above Bash script.
7. Configure other settings as needed (network, storage, etc.).
8. Add a security group rule to allow HTTP (port 80) and HTTPS (port 443) traffic.
9. Launch the instance.

## Step 3: Set Up a Custom Domain with AWS Route 53 (Optional) or any domain provider you want

1. Purchase a domain name from any domain registrar (or use AWS Route 53).
2. In AWS Route 53, create a hosted zone for your domain.
3. Add an **A Record** in Route 53 to point to your EC2 instance's public IP address.

Example:

```bash
your-domain.com --> <EC2 Public IP>
```

## Step 4: Install Certbot and Obtain SSL Certificates

To secure your domain with SSL, we will use **Certbot** to obtain free certificates from **Let’s Encrypt**.

1. SSH into your EC2 instance:

```bash
ssh -i your-key.pem ubuntu@<public-ip-address>
```

2. Install Certbot and dependencies:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

3. Run Certbot to obtain SSL certificates for your domain:

```bash
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```
![Screenshot 2024-10-20 163905](https://github.com/user-attachments/assets/fa586a4c-a284-471a-bfc0-0322951b3170)
![Screenshot 2024-10-20 205005](https://github.com/user-attachments/assets/15e3da62-970f-47d0-8655-3e310fa9d4b8)
![Screenshot 2024-10-20 175925](https://github.com/user-attachments/assets/a2b9a96e-4171-4711-8502-fba3a9062a31)
![Screenshot 2024-10-20 165351](https://github.com/user-attachments/assets/9515a79f-98af-466e-a789-a781fc371309)
![Screenshot 2024-10-20 164016](https://github.com/user-attachments/assets/bf02ba9e-fcd5-4623-92c8-f38e97a8f684)
![Screenshot 2024-10-20 163957](https://github.com/user-attachments/assets/5ce5da04-d8fe-4ec2-9e5c-279bd8767bfa)

This command will:
- Obtain the SSL certificate from Let’s Encrypt
- Automatically configure NGINX to use the certificates
- Redirect HTTP to HTTPS

4. Follow the prompts to complete the SSL installation.

## Step 5: Verify SSL Installation

To confirm that SSL has been correctly configured:

1. Open your browser and navigate to `https://your-domain.com`.
2. Verify that the connection is secure (a padlock icon should appear).

You can also check the NGINX SSL configuration:

```bash
sudo nginx -t
```

## Step 6: Automatic SSL Certificate Renewal

Certbot sets up automatic renewal for your certificates. To test the renewal process, run:

```bash
sudo certbot renew --dry-run
```

## Step 7: Update Your NGINX Configuration (Optional)

To further customize your NGINX server block, you can modify the configuration file:

```bash
sudo nano /etc/nginx/sites-available/default
```

Example configuration for your domain:

```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    root /var/www/html;

    location / {
        try_files $uri $uri/ =404;
    }

    listen [::]:443 ssl ipv6only=on; 
    listen 443 ssl; # managed by Certbot

    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}
```

Restart NGINX after making any changes:

```bash
sudo systemctl restart nginx
```
 You can also congigure everything on the domain name site 
 ![Screenshot 2024-10-21 105601](https://github.com/user-attachments/assets/afed386d-0efc-4876-b183-e705b2110caf)
![Screenshot 2024-10-21 104933](https://github.com/user-attachments/assets/64cd387b-02e9-4395-a5b1-e231f2d47b7c)

or Install certbot and include the domain name of the on while configuring it
![Screenshot 2024-10-21 105739](https://github.com/user-attachments/assets/261759f6-c63f-4145-8eee-9dad459f48a5)
![Screenshot 2024-10-21 105607](https://github.com/user-attachments/assets/374cc807-0ffa-4b6b-9e8d-9880c6db6952)

check the ip address of your server to confirm the configuration

![Screenshot 2024-10-21 105819](https://github.com/user-attachments/assets/1fd69029-544a-4a37-bcc2-67b18559ccbc)
![Screenshot 2024-10-21 105800](https://github.com/user-attachments/assets/c2b1a4e8-5668-49dd-8029-976b55dbe5b1)
