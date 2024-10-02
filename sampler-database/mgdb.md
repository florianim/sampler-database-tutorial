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

### Step 3: Start and Verify MongoDB Service

Start the MongoDB service and check its status:

`sudo systemctl start mongod`{{exec}}

Install the mongodb-clients

`sudo apt install mongodb-clients`{{exec}}

### Step 4: Create a Test Database and User in MongoDB

To create a test database and user, open the MongoDB shell:

`mongo`{{exec}}

Then, run the following commands inside the MongoDB shell:


`use test_db`{{exec}}

`db.createUser({user: "test_user", pwd: "password", roles: [{role: "readWrite", db: "test_db"}]})`{{exec}}



### Step 5: Exit the MongoDB Shell

`exit`{{exec}}