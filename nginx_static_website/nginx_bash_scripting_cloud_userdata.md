# NGINX Web Server Setup on AWS EC2 Using Bash Script as User Data

This guide demonstrates how to create and launch an AWS EC2 instance with an NGINX web server using a Bash script provided as user data.

## Prerequisites

Before proceeding, ensure you have the following:

- AWS Account
- AWS CLI configured with appropriate permissions
- Basic knowledge of EC2 and NGINX
- SSH access to EC2 instances

## Step 1: Create a Bash Script for NGINX Installation

The Bash script will automate the installation of NGINX and start the web server. This script will be added as EC2 user data.

```bash
#!/bin/bash

##########################################################################
## Genius B
## 18 Oct 2024
## Automate Apache and NGINX installation on an instance
## - Installs Apache and NGINX
## - Enables and restarts both services
## - Updates system packages
##########################################################################

# Function to print messages in green
green_echo() {
    echo -e "\e[32m$1\e[0m"
}

# Function to print messages in red
red_echo() {
    echo -e "\e[31m$1\e[0m"
}

# Function to update and upgrade system packages
install_apt_software_packages() {
    echo "=========================*=========================="
    green_echo "Software update: "
    echo "=========================*=========================="
    if sudo apt-get update && sudo apt-get upgrade -y; then
        green_echo "System update successful."
    else
        red_echo "Update failed. Check documentation for correct code if unsure."
        exit 1
    fi
}

# Function to install Apache2
install_software_packages() {
    echo "=========================*=========================="
    green_echo "Installing Apache2: "
    echo "=========================*=========================="
    if sudo apt-get install apache2 -y; then
        green_echo "Apache2 installation successful."
    else
        red_echo "Apache2 installation failed."
        exit 1
    fi
}

# Function to enable Apache2 service
enable_services() {
    echo "=========================*=========================="
    green_echo "Enabling Apache2: "
    echo "=========================*=========================="
    if sudo systemctl enable apache2; then
        green_echo "Apache2 enabled successfully."
    else
        red_echo "Failed to enable Apache2."
        exit 1
    fi
}

# Function to restart Apache2 service
restart_services() {
    echo "=========================*=========================="
    green_echo "Restarting Apache2: "
    echo "=========================*=========================="
    if sudo systemctl restart apache2; then
        green_echo "Apache2 restarted successfully."
    else
        red_echo "Failed to restart Apache2."
        exit 1
    fi
}

# Function to install NGINX
install_nginx() {
    echo "=========================*=========================="
    green_echo "Installing NGINX: "
    echo "=========================*=========================="
    if sudo apt-get install nginx -y; then
        green_echo "NGINX installation successful."
    else
        red_echo "NGINX installation failed."
        exit 1
    fi
}

# Function to enable NGINX service
enable_nginx() {
    echo "=========================*=========================="
    green_echo "Enabling NGINX: "
    echo "=========================*=========================="
    if sudo systemctl enable nginx; then
        green_echo "NGINX enabled successfully."
    else
        red_echo "Failed to enable NGINX."
        exit 1
    fi
}

# Function to restart NGINX service
restart_nginx() {
    echo "=========================*=========================="
    green_echo "Restarting NGINX: "
    echo "=========================*=========================="
    if sudo systemctl restart nginx; then
        green_echo "NGINX restarted successfully."
    else
        red_echo "Failed to restart NGINX."
        exit 1
    fi
}

# Main logic: call functions to install, enable, and restart Apache2 and NGINX
install_apt_software_packages
install_software_packages
enable_services
restart_services
install_nginx
enable_nginx
restart_nginx
```
Remenber to always test scripts before you use them to deploy on the cloud
![Screenshot 2024-10-18 120530](https://github.com/user-attachments/assets/56845b52-efbc-4b43-b8e3-a4d4088234b1)
![Screenshot 2024-10-18 120517](https://github.com/user-attachments/assets/d7c495be-88a7-4f25-8f12-bd3efb1a2bcc)

After testing, i used it to create an instance
![Screenshot 2024-10-18 122345](https://github.com/user-attachments/assets/cb7dd997-68f7-4f28-bbc8-812ed207e46e)

SSH into it to enable Nginx
![Screenshot 2024-10-18 124903](https://github.com/user-attachments/assets/85376ddc-d283-4fcb-a003-92456fddd784)
![Screenshot 2024-10-18 122408](https://github.com/user-attachments/assets/69610564-e835-49b3-833d-83240acae53c)

add a static website to the instance
![Screenshot 2024-10-18 124912](https://github.com/user-attachments/assets/bc1830e8-88f7-46cf-bfd3-1a669b769f28)
![Screenshot 2024-10-18 125518](https://github.com/user-attachments/assets/c3cc2dae-e6d2-4f5b-b152-e476c4a8462a)
![Screenshot 2024-10-18 125703](https://github.com/user-attachments/assets/7e05ccfe-be2f-4353-87a7-070fbdcd6279)
![Screenshot 2024-10-18 123900](https://github.com/user-attachments/assets/80a63707-afc4-4417-a429-55afe9f3a7cb)

Paste the ip of your instance n the browser to check your website

![Screenshot 2024-10-18 130311](https://github.com/user-attachments/assets/b22efb87-1eaf-48ac-9fb7-fea8cc288c9a)
![Screenshot 2024-10-18 130330](https://github.com/user-attachments/assets/bf696658-d78c-43a8-8457-07ec412e6d7d)
![Screenshot 2024-10-18 130340](https://github.com/user-attachments/assets/6b77a443-d2dd-44b3-94ed-cce3a28497ea)
![Screenshot 2024-10-18 130350](https://github.com/user-attachments/assets/08a16fa2-aff7-466c-8325-4763bedf7659)
![Screenshot 2024-10-18 130400](https://github.com/user-attachments/assets/f4ef5c44-cdf4-4293-b9c8-7d7e3875235e)
![Screenshot 2024-10-18 130408](https://github.com/user-attachments/assets/d14f68c4-06ea-4921-8d2c-66eb2ed752cf)

You have successfully deployed a static website
