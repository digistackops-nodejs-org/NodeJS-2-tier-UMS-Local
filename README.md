# Install Mongo DB
## Launch EC2 "t2.micro" Instance and In Sg, Open port "27017" for MongoDB
### Create mondDB repo in YUM repository
```
sudo vim /etc/yum.repos.d/mongodb-org-8.0.repo
```
### Add MongoDB repo Details 
```
[mongodb-org-8.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2023/mongodb-org/8.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://pgp.mongodb.com/server-8.0.asc
```
### Install mongoDB
```
sudo yum update -y
sudo yum install -y mongodb-org
```
### Start mongoDB
```
sudo systemctl daemon-reload
sudo systemctl enable mongod
sudo systemctl start mongod
sudo systemctl status mongod
```
## Setup MongoDB

#### Allow Remote Access
```
sudo vim /etc/mongod.conf
```
Replace 0.0.0.0 in bindIp
```
# network interfaces 
    net:   
       port: 27017   
       bindIp: 0.0.0.0 # to bind to all interfaces
```
##### Restart mongoDB
```
sudo systemctl restart mongod
```
### Use mongo-compass in your Local Machine and try to access your MongoDB
```
mongodb://<your-AWS-Public-IP>:27017
```
### Using Mongo-Compose create DB "user-account" and Collection "users"

<img width="1103" height="528" alt="image" src="https://github.com/user-attachments/assets/4ad6e0a7-bd5d-4bf3-9068-69eb63e18bd4" />
<img width="577" height="432" alt="image" src="https://github.com/user-attachments/assets/bc64c51c-765d-42d0-afef-a9fe788faa01" />
<img width="1383" height="382" alt="image" src="https://github.com/user-attachments/assets/9ca73003-bb27-4d0c-a9d5-ec1154b70608" />



### Login to your mongoDB and Create application user
```
mongosh
```
### switch to admin user

```
use admin
```

Switch to employeedb 

```
use user-account
```

### Create Application User

```
db.createUser({
  user: "appuser",
  pwd: "pa55Word",
  roles: [
    { role: "readWrite", db: "user-account" }
  ]
});
```

# Setup Application Server

## Note ==> HERE in our PROD Branch Code we alredy Edit these Code in "server.js", so no need to Change any thing HERE

### Good-To-Know
```
As of Now We Hardcode the DB Credentials in "server.js"

In "server.js" we mention our DB credentials Manually and push it to GIT. 
We have 2 problems HERE
1. These code is not eligibile for CICD, we manually enter the Credentials
2. It expose our Credentials to everyone

Which is Not recommended in PROD as well

So we need to Pass our DB Credentials as Environment Variables
For that we need to Change our Code 

Open your "server.js" Edit MongoDB configuration

You see like these

----
// use when starting application locally
let mongoUrlLocal = "mongodb://appuser:pa55Word@<Private-IP>:27017";
---


So we need to Edit these code as per Environment Variables

----
require('dotenv').config();

let mongoUrlLocal = `mongodb://${process.env.MONGO_USER}:${process.env.MONGO_PASS}` +
                    `@${process.env.MONGO_HOST}:27017`;

let databaseName = process.env.MONGO_DB;
----
```

## Launch EC2 "t2.micro" Instance and In Sg, Open port "3000" for Node.js Application server


### Install Node
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 16
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install --lts
nvm use --lts
```
### Check Node Version
```
node -v
npm -v
```
### Install Git
```
sudo yum install git -y
```
#### To start this application first you can get the code using below url
##### Clone the Repo
```
sudo git clone https://github.com/digistackops-nodejs-org/NodeJS-2-tier-UMS-Local.git
cd NodeJS-2-tier-UMS-Local
```
##### Switch to Local-setup Branch
```
sudo git checkout 02-Local-setup-Prod
```

### Pass our DB Credentials as Environment Variables 

```
export MONGO_USER=appuser
export MONGO_PASS=pa55Word
export MONGO_HOST=AWS-DB-Private-IP
export MONGO_DB=user-account
```
##### if you get Permission Issue then 

```
sudo chown -R ec2-user:ec2-user /home/ec2-user/NodeJS-2-tier-UMS-Local
```
#### Download the Dependencies
```
npm install
```

#### Start the Application
```
node server.js
```
#### Access Your Application in Browser
```
http://<Your-AWS-Public-IP>:3000
```

<img width="455" height="579" alt="image" src="https://github.com/user-attachments/assets/7b0356a7-652a-4d19-a3bf-e8aac40329d7" />

#### Check Data in DB
Login to mongo-compass check user-account DB and user collection

<img width="966" height="167" alt="image" src="https://github.com/user-attachments/assets/8eb0ce55-8487-4a48-a7c2-d031acf5d192" />
