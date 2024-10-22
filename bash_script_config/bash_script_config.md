
# Automate Software Installation and Configuration Using a Bash Script

This guide demonstrates how to write a Bash script to automate the installation of software packages, configure them by editing configuration files, and restart necessary services. This approach simplifies software deployment, especially on multiple servers.

## Script Overview

The script performs the following tasks:

1. Installs a list of software packages using the system's package manager (e.g., `apt` for Ubuntu/Debian).
2. Configures the installed software by modifying configuration files.
3. Restarts services to apply the new configurations.

## Prerequisites

- A Linux server (e.g., Ubuntu or Debian).
- Root or `sudo` access to the server.
- Basic knowledge of Bash scripting.

## Sample Bash Script

Here's a sample Bash script that installs and configures software packages, followed by restarting the necessary services.
![Screenshot 2024-10-18 112104](https://github.com/user-attachments/assets/1849145b-2f91-472b-a236-9d05e09f9a3a)
```bash
#!/bin/bash

##########################################################################
## Genius B
## 18 Oct 2024
## Automate software installation and configuration
## - Create a script that:
##   - Installs a list of software packages using the system's package manager
##   - Configures the installed software by editing configuration files
##   - Restarts services if necessary
##########################################################################

# Function to print messages in green
green_echo() {
    echo -e "\e[32m$1\e[0m"
}

# Function to print messages in red
red_echo() {
    echo -e "\e[31m$1\e[0m"
}

# Function to install software packages
install_software_packages() {
    echo "=========================*=========================="
    green_echo "Installing Apache2: "
    echo "=========================*=========================="

    if sudo apt-get install apache2 -y; then
        green_echo "Apache2 installation successful."
    else
        red_echo "Installation failed. Check documentation for correct code if unsure."
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

# Main logic: call functions to install, enable, and restart Apache2
install_software_packages
enable_services
restart_services

```

### How the Script Works

- **Install Packages**: The `install_packages()` function installs a list of predefined software packages using `apt`. It checks if a package is already installed to avoid reinstalling.
  
- **Configure Software**: The `configure_software()` function modifies the configuration files of the installed software. For example, the script updates the NGINX default configuration to change the default homepage.

- **Restart Services**: The `restart_services()` function ensures that the services are restarted after configuration changes to apply them.

## Usage Instructions

1. **Copy the Script**: Copy the above Bash script into a file named `install_and_configure.sh`.

2. **Make the Script Executable**:

   ```bash
   chmod +x install_and_configure.sh
   ```

3. **Run the Script**:

   ```bash
   ./install_and_configure.sh
   ```

4. The script will install the software, configure it, and restart the necessary services.

## Customization

You can modify the script to fit your specific needs:

- Add more software packages to the `SOFTWARE_PACKAGES` array.
- Modify the `configure_software()` function to adjust configurations of other software.
- Add or remove services to be restarted in the `restart_services()` function.

## Conclusion

This script provides an efficient way to automate software installation, configuration, and service management in a Linux environment. By modifying the script, you can easily customize it for other software and configurations.

---
![Screenshot 2024-10-18 111423](https://github.com/user-attachments/assets/fa095123-5b5e-4c8b-8986-70025b141b9b)
![Screenshot 2024-10-18 112046](https://github.com/user-attachments/assets/93d26134-757c-41f8-8b35-895882d8c155)
![Screenshot 2024-10-18 112034](https://github.com/user-attachments/assets/330473c1-79a8-4722-9a49-5d89380c4d0e)
