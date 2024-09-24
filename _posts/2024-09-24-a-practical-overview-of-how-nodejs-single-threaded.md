---
title: Node.js Event Loop Explained
date: 2021-03-02 10:45:37 -05:00
layout: post
permalink: blog/vpn-setup-with-pritunel/
categories: Tutorial
author: Dinesh Agrawal 
coverImage: https://dz2cdn1.dzone.com/storage/temp/16193208-1662525593978.png
---

__This article covers the main components of Node.js with practical examples so that people can actually run the snippet and play with it__

#### Overview

This documentation provides a comprehensive guide to setting up a Virtual Private Network ([VPN](https://dzone.com/articles/the-influence-of-vpn-on-software-development)) server using Pritunl, a popular [open-source](https://dzone.com/articles/what-is-open-source) VPN server management platform. By following these steps, users can establish a secure and private network infrastructure suitable for various applications, such as remote access, secure communication, and data privacy.

#### Prerequisites

*   **AWS account:** Users must have an active AWS (Amazon Web Services) account to create and manage instances on the cloud platform.
*   **Instance creation:** An EC2 instance needs to be created in the desired region (e.g., Mumbai) using the Ubuntu 22.04 AMI. Users should ensure that ports 80, 443, and 22 are allowed in the security group settings.
*   **Access to the instance: **Users need SSH access to the instance created on AWS. They should possess the private key associated with the [EC2 instance](https://dzone.com/articles/how-to-understand-and-choose-your-first-ec2-instan) to establish a secure connection.
*   **Web browser:** A web browser, preferably Google Chrome, is required to access the Pritunl dashboard and configure the VPN server settings.
*   **Pritunl client: **To connect to the VPN server from client machines, users must install the Pritunl client application. The client can be installed on various operating systems, including Windows, macOS, and Linux distributions.
*   **Network configuration: **Users should ensure proper network configuration, including DNS settings, virtual network details, and firewall rules, to establish seamless communication between the VPN server and client machines.
 
#### Setting up a VPN Server

##### Step 1: Setup Instance 

*   Launch an instance in the Mumbai region using the Ubuntu 22.04 AMI.
*   Ensure ports 80, 443, and 22 are allowed in the security group.

__Steps for Launching Instance__

*   Select the Mumbai region.
*   Launch an instance, providing a name.
*   Choose the specified AMI.  
    ![quick start](https://dz2cdn1.dzone.com/storage/temp/17548440-1709639778419.png)
*   Select t2.micro instance type
*   Choose or create a key pair
*   Select the default VPC and adjust network settings if necessary
*   Allow SSH, HTTP, and HTTPS ports in the security group
*   Launch the instance
*   
#### Step 2: Access the Instance and Setup VPN

*   SSH into the instance: ssh ubuntu@yourip -i "key"
*   Switch to root: sudo -i
*   Execute the following commands
*   

```powershell
sudo tee /etc/apt/sources.list.d/pritunl.list << EOF

deb http://repo.pritunl.com/stable/apt jammy main

EOF



# Import signing key from keyserver

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A

# Alternative import from download if keyserver offline

curl https://raw.githubusercontent.com/pritunl/pgp/master/pritunl_repo_pub.asc | sudo apt-key add -



sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list << EOF

deb https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse

EOF



wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -



sudo apt update

sudo apt --assume-yes upgrade



# WireGuard server support

sudo apt -y install wireguard wireguard-tools



sudo ufw disable



sudo apt -y install pritunl mongodb-org

sudo systemctl enable mongod pritunl

sudo systemctl start mongod pritunl
```

*   Respond with Enter twice when prompted during the process

![Respond with Enter twice when prompted during the process](https://dz2cdn1.dzone.com/storage/temp/17548441-1709639890736.png)

- Now, Execute the following commands:

```powershell
sudo sh -c 'echo "* hard nofile 64000" >> /etc/security/limits.conf'

sudo sh -c 'echo "* soft nofile 64000" >> /etc/security/limits.conf'

sudo sh -c 'echo "root hard nofile 64000" >> /etc/security/limits.conf'

sudo sh -c 'echo "root soft nofile 64000" >> /etc/security/limits.conf'
```

#### Step 3: Access the App via Chrome Browser From Your System

*   Go to the Chrome dashboard and enter the [URL](https://yourip/) as follows
*   Proceed to t

![not secure](https://dz2cdn1.dzone.com/storage/temp/17548442-1709639964744.png)

#### Step 4: Generate Setup Key

*   Run the command: ``pritunl setup-key``

![](https://dz2cdn1.dzone.com/storage/temp/17548447-1709639998846.png)

* Get the setup key from the command output.

#### Step 5: Setup VPN

*   Enter the setup key in the dashboard and save.
*   Ensure port 27017 is allowed in the security group.
*   

![pritunl](https://dz2cdn1.dzone.com/storage/temp/17548450-1709640029856.png)

Then again, this page will show.

Proceed with the connection again

![Proceed with the connection again.](https://dz2cdn1.dzone.com/storage/temp/17548451-1709640057271.png)

#### Step 6: View Default Password

* To view the username and password, execute the comm
 `pritunl default-password`

![](https://dz2cdn1.dzone.com/storage/temp/17548454-1709640088655.png)

* Copy the credentials from the output and sign in to the online interface.

![sign in](https://dz2cdn1.dzone.com/storage/temp/17548455-1709640103826.png)

After that, you'll be sent to the setup screen.


![initial setup](https://dz2cdn1.dzone.com/storage/temp/17548458-1709640128793.png)

Click the Save button after making any necessary changes to your username or password. This is the page that you should view.

#### Step 7: Add Organization

* Click on the "Add Organization" button in the user section.

![add organization](https://dz2cdn1.dzone.com/storage/temp/17548462-1709640192232.png)

* Enter the organization name and press Add.

![add](https://dz2cdn1.dzone.com/storage/temp/17548464-1709640221788.png)

#### Step 8: Add User

* Select the "Add User" option.
![myorg](https://dz2cdn1.dzone.com/storage/temp/17548465-1709640255691.png)

* Enter username, select PIN, and press Add.

![add user](https://dz2cdn1.dzone.com/storage/temp/17548466-1709640276748.png)

#### Step 9: Setup Server

* Select the Servers tab.

![users and organizations](https://dz2cdn1.dzone.com/storage/temp/17548468-1709640315803.png)

The page you should view is this one:

* Click on "Add Server" option. 
![servers](https://dz2cdn1.dzone.com/storage/temp/17548469-1709640358112.png)

The page you should view is this one:

*   Enter DNS IP, virtual network, port, and server name. Click Add.
*   Ensure the specified port is allowed in the security group.
*   
![](https://dz2cdn1.dzone.com/storage/temp/17548470-1709640393690.png)

#### Step 10: For Attach Organisation 

The page you should view is this one:

* Click on the "Attach Organization" option to connect the organization to the server.

![attach organization](https://dz2cdn1.dzone.com/storage/temp/17548471-1709640417012.png)
This is the page that you ought to view:

![attach organization](https://dz2cdn1.dzone.com/storage/temp/17548472-1709640432494.png)

#### Step 11: Start Server

The page you should view is this one:

* Click on the "Start Server" button to launch the VPN server.

![Click on "Start Server" button to launch the VPN server.](https://dz2cdn1.dzone.com/storage/temp/17548473-1709640454642.png)

The page you should view is this one:

![serverss](https://dz2cdn1.dzone.com/storage/temp/17548474-1709640481274.png)

#### Setting up Client Machine 

#### Step 12: Setup Client Machine

* Install the Pritunl client or launch a new instance.
* During installation, type 'y' when prompted.

```powershell
sudo tee /etc/apt/sources.list.d/pritunl.list << EOF

deb https://repo.pritunl.com/stable/apt jammy main

EOF



sudo apt --assume-yes install gnupg

gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 7568D9BB55FF9E5287D586017AE645C0CF8E292A

gpg --armor --export 7568D9BB55FF9E5287D586017AE645C0CF8E292A | sudo tee /etc/apt/trusted.gpg.d/pritunl.asc

sudo apt update

sudo apt install pritunl-client-electron
```

#### Step 13: Download User Profile

* Download the user profile.

![Download the user profile.](https://dz2cdn1.dzone.com/storage/temp/17548476-1709640538309.png)

#### Step 14: Access Pritunl Client

* Access the Pritunl client application from the Application menu.
* Browse and attach the user profile.

![](https://dz2cdn1.dzone.com/storage/temp/17548477-1709640567482.png)

* Connect and provide the PIN created in Step 8.
* Ensure proper connection establishment.
  
![](https://dz2cdn1.dzone.com/storage/temp/17548479-1709640591883.png)

![](https://dz2cdn1.dzone.com/storage/temp/17548480-1709640609196.png)










