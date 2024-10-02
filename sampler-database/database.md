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

If you see something like this, it proves that your installation was successful.

![Mysql Status](./image/mysql.png)


### Step 5: Log in to MySQL

You can now log in to MySQL using the root account:

`sudo mysql -u root -p`{{exec}}

Press enter for no password

### Step 6: Create a Test Database and User

Inside the MySQL shell, you can create a test database and a new user:


`CREATE DATABASE test_db;`{{exec}}

`CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'password';`{{exec}}

`GRANT ALL PRIVILEGES ON test_db.* TO 'test_user'@'localhost';`{{exec}}

`FLUSH PRIVILEGES;`{{exec}}

`EXIT;`{{exec}}

## Installing PostgreSQL

PostgreSQL is a powerful, open-source object-relational database system. Let’s move on to installing PostgreSQL.

### Step 1: Install PostgreSQL

`sudo apt install postgresql postgresql-contrib -y`{{exec}}

### Step 2: Start and Check PostgreSQL Service

`sudo systemctl start postgresql`{{exec}}

`sudo systemctl status postgresql`{{exec}}

If you see something like this, it proves that your installation was successful.

![Mysql Status](./image/pgsql.png)

### Step 3: Switch to PostgreSQL User

In PostgreSQL, you typically interact with the database using the postgres user. Switch to the PostgreSQL user with this command:

`sudo -i -u postgres`{{exec}}

### Step 4: Access PostgreSQL Shell

Access the PostgreSQL shell by typing:

`psql`{{exec}}

### Step 5: Create a Test Database and User

Inside the PostgreSQL shell, create a new database and user:


`CREATE DATABASE test_db;`{{exec}}

`CREATE USER test_user WITH PASSWORD 'password';`{{exec}}

`GRANT ALL PRIVILEGES ON DATABASE test_db TO test_user;`{{exec}}

`\q`{{exec}}



### Step 6: Exit the PostgreSQL User Session

`exit`{{exec}}


## Installing MongoDB

### Step 1: Add MongoDB Repository

First, we need to add the MongoDB repository to the system. Run the following commands to do that:


`sudo apt update`{{exec}}

`sudo apt install gnupg -y`{{exec}}

`wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -`{{exec}}

`echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list`{{exec}}



### Step 2: Install MongoDB

After adding the repository, install MongoDB using the following command:

`sudo apt update`{{exec}}

`sudo apt install -y mongodb-org`{{exec}}

`sudo apt install mongodb-clients`{{exec}}

### Step 3: Start and Verify MongoDB Service

Start the MongoDB service and check its status:

`sudo systemctl start mongod`{{exec}}

### Step 4: Create a Test Database and User in MongoDB

To create a test database and user, open the MongoDB shell:

`mongo`{{exec}}

Then, run the following commands inside the MongoDB shell:


`use test_db`{{exec}}

`db.createUser({user: "test_user", pwd: "password", roles: [{role: "readWrite", db: "test_db"}]})`{{exec}}



### Step 5: Exit the MongoDB Shell

`exit`{{exec}}





