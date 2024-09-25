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

# Configure PostgreSQL

# 1. Set password for ssh postgres user and admin postgres database password

For security, set a strong password for the system user and default database admin user account. Use the following commands:

```bash
# Change the ssh user password:
sudo passwd postgres

# Log in using the Postgres system account:
su - postgres

# Now, change the admin database password:
psql -c "ALTER USER postgres WITH PASSWORD 'your-password';"
exit
```
![12](https://github.com/user-attachments/assets/89e50768-1392-4e33-a284-8e5f4cfe527c)

![13](https://github.com/user-attachments/assets/f6a432de-9575-4db5-8265-1a9afb78842a)

```bash
sudo cp /var/lib/pgsql/data/postgresql.conf /var/lib/pgsql/data/postgresql.conf.bck
```

![14](https://github.com/user-attachments/assets/c14c5950-83d5-4984-a1ae-bf3910e43d7c)

Edit this file with a text editor:

```bash
sudo vi /var/lib/pgsql/data/postgresql.conf
```

![15](https://github.com/user-attachments/assets/647beddf-c333-430b-9a6f-ef6780ce33ea)

By default, PostgreSQL only listens to localhost


```bash
listen_addresses = 'localhost'
```

if you want to listen all IP addresses:

```bash
listen_addresses = '*' # what IP address(es) to listen on;
```

![16](https://github.com/user-attachments/assets/78b1c78d-6375-4f7a-bbee-a31448a36933)

![17](https://github.com/user-attachments/assets/24297afe-1f0a-4711-a62a-17ef3477f867)

# Authentication

For authentication, there is a separate file called pg_hba.conf in the same directory as the primary configuration file.

Before making any changes, back up the configuration file:

```bash
sudo cp /var/lib/pgsql/data/pg_hba.conf /var/lib/pgsql/data/pg_hba.conf.bck
```

![18](https://github.com/user-attachments/assets/86f8cdbe-6963-439c-89a6-d99dfa188876)

Edit this file with a text editor:

```bash
sudo sed -i 's/ident$/md5/' /var/lib/pgsql/data/pg_hba.conf
```

![19](https://github.com/user-attachments/assets/fb7f6106-5ac5-4b9e-a989-4a0425e69b9e)

To apply all the changes, restart the PostgreSQL service using the following command.

```bash
sudo systemctl restart postgresql
```

![20](https://github.com/user-attachments/assets/08e0a40d-2099-4fa5-9080-7f9ba825d658)

# How to Create a User & Database

Use this section to create a new user and database on PostgreSQL:

```bash
# Connect to the PostgreSQL server as the Postgres user:
sudo -i -u postgres psql

# Create a new database user:
CREATE USER yourusername WITH PASSWORD 'password';

# Create a new database:
CREATE DATABASE database_name;

# Grant all privileges on the database to the user:
GRANT ALL PRIVILEGES ON DATABASE database_name TO yourusername;

# To list all available PostgreSQL users and databases:
\l
```

![21](https://github.com/user-attachments/assets/722108e8-0d75-4ee2-a3c9-2f944e9528a1)

# Accessing the Database

You can access the database you created using the PostgreSQL client command psql. Local or SSH server connection users can use the following syntax:

```bash
psql -h localhost -U username -d database_name
```
![22](https://github.com/user-attachments/assets/bef24462-19e0-4639-863e-76ea2a8e6367)

Remote users can use:

```bash
psql -h server-ip-address -U username -d database_name
```
![23](https://github.com/user-attachments/assets/b4a22b82-5657-4550-adee-fa4fc7b38bd0)

Replace username with the user you created and database_name with the name of the database assigned to that user.

Access the postgres account on your server by typing:

```bash
sudo -i -u postgres
```

Now you can immediately access Postgres prompt by typing:

```bash
$ psql
postgres.

# To list the databases:
postgres=# \l

# You can exit Postgres prompt by typing:
postgres=# \q
```
![24](https://github.com/user-attachments/assets/aff55a30-1b7c-45bb-8158-80d43fdb27f6)

To access your PostgreSQL database from external sources, make sure to configure your Amazon Linux EC2 security group to allow incoming traffic on port 5432, which is the default port used by PostgreSQL


#Step 1: Stop PostgreSQL Service

Before uninstalling PostgreSQL, ensure that the service is stopped:

```bash
sudo systemctl stop postgresql
```

# Step 2: Disable PostgreSQL Service

Prevent PostgreSQL from starting at system boot:

```bash
sudo systemctl disable postgresql
```

# Step 3: Remove PostgreSQL Packages


Use the following commands to remove PostgreSQL 15 packages. These are the same packages you installed during the installation process:

```bash
sudo dnf remove postgresql15.x86_64 postgresql15-server
```

# Step 4: Completely Remove PostgreSQL

After removing the packages, delete all PostgreSQL-related configuration files and data using the following command:

```bash
sudo rm -rf /var/lib/pgsql /var/log/postgresql /etc/postgresql
```

# Conclusion

With these steps, you have successfully uninstalled PostgreSQL 15 from your Amazon Linux 2023 instance. Make sure you have backed up any critical data before proceeding with the uninstallation process.



# Congratulations Your PRoject is run ....

# DELETE AND UNSTALL EC2 INSTANCE AND POSTGRESQL SERVER 
