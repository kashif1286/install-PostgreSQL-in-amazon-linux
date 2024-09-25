# Introduction

PostgreSQL is a powerful open-source relational database management system widely used for storing and managing data. If you’ve recently installed Amazon Linux 2023 on your AWS EC2 instance and need guidance on how to install PostgreSQL 15, you’re in the right place.

![Blank board - Page 1](https://github.com/user-attachments/assets/c2a6b454-e983-4e46-b39c-d605e2f9f7fd)


 # Prerequisites:

Before you start, ensure you meet the following prerequisites:

An AWS EC2 instance running Amazon Linux 2023 with administrator privileges.

A minimum of 1GB of available hard disk space, 2GB of RAM, and a single-core CPU.

# Step 1- Launching and Configuring Your EC2 Instance

# 1. Log in to AWS services and select EC2.

![1](https://github.com/user-attachments/assets/0ae0a494-e759-42d9-9ed5-fff306663851)

![2](https://github.com/user-attachments/assets/11c9e25d-8825-4f82-a122-07836f90659f)

You can click to “launch instance” button for create a EC2 instance

#  Configure the instance name and the OS image as follows

![3](https://github.com/user-attachments/assets/0ce98ea5-783a-42ab-b2f5-25c69c6f3cd5)

# Scroll down and configure the instance type and key pair

![4](https://github.com/user-attachments/assets/8fa35f04-c676-4048-b1b6-930b0ef9b13c)
#  Modify to disk size as 20 GB
![5](https://github.com/user-attachments/assets/821ac027-c872-49cb-bfbe-00b049ea3f36)

# Accessing Your EC2 Instance
Server is ready now select the server and click the connect button

![6](https://github.com/user-attachments/assets/2ea21b76-6ce0-411e-b40e-b5998ac1ed6f)

![7](https://github.com/user-attachments/assets/7a08ff0d-ac0f-41a5-bfc2-d1f345f0ea69)

Congratulations! You have successfully set up EC2 Instance Connect


Server is ready we can install the Postgresql . Please follow the below steps.

# Update Amazon Linux 2024 Packages

Before installing any packages, it’s essential to update your system to ensure you have the latest updates and refresh the DNF package cache. Open your terminal or connect to your Amazon Linux instance via SSH and run the following command:

```bash
sudo dnf update
```

![8](https://github.com/user-attachments/assets/74608983-8830-4fb6-bd9d-dde896c3ebb3)

# Installing PostgreSQL

```bash
sudo dnf install postgresql15.x86_64 postgresql15-server -y
```
![9](https://github.com/user-attachments/assets/21a992de-91a1-4aad-a758-3640f84f49b0)

# Initializing PostgreSQL Database

Before starting and enabling the database service, let’s initialize it. Use the initdb command, which will create a new PostgreSQL database cluster referring to a collection of databases managed by a single server instance:

```bash
sudo postgresql-setup --initdb
```
![10](https://github.com/user-attachments/assets/f3465e49-63f3-47c3-b4ca-c48008c9e7df)

# Starting and Enabling PostgreSQL Service

After completing the initialization, start and enable the PostgreSQL server service, so it starts automatically with system boot:
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo systemctl status postgresql
```
![11](https://github.com/user-attachments/assets/8d2c45a5-4099-41a9-a3f1-528641c66644)
