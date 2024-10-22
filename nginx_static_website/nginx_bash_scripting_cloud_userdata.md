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
