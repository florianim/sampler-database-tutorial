# Installing MySQL, PostgreSQL, and MongoDB on Ubuntu

In this tutorial, we will guide you through installing MySQL, PostgreSQL, and MongoDB step-by-step on an Ubuntu environment. Follow along to install each of these databases.

---

## Installing MySQL

MySQL is a popular relational database management system used for web and server applications. Let’s begin by installing MySQL.

### Step 1: Update the package list

First, ensure that your package list is updated by running the following command:


`sudo apt update`{{exec}}

### Step 2: Install MySQL server using the following command:

`sudo apt install mysql-server -y`{{exec}}


### Step 3: Secure MySQL Installation

Once MySQL is installed, run the security script to set a root password and configure security settings.

`sudo mysql_secure_installation`{{exec}}

Follow the prompts to set up a root password and configure security options (it's recommended to answer Y to most of the options).

### Step 4: Start and Verify MySQL Service

Ensure that the MySQL service is running properly by starting it and checking its status:

`sudo systemctl start mysql`{{exec}}

`sudo systemctl status mysql`{{exec}}

### Step 5: Log in to MySQL
You can now log in to MySQL using the root account:
`sudo mysql -u root -p`{{exec}}

### Step 6: Create a Test Database and User
Inside the MySQL shell, you can create a test database and a new user:

```
CREATE DATABASE test_db;
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON test_db.* TO 'test_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```{{exec}}





