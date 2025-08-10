# Install Mongo DB
## Launch EC2 "t2.micro" Instance and In Sg, Open port "27017" for MongoDB & "3000" for Node.js Application server
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
### Install Node
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 16
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
sudo git clone https://github.com/techizone-Small-Project-org/NodeJS-2-tier-UMS-App.git
cd NodeJS-2-tier-UMS-App
```
##### Switch to Local-setup Branch
```
sudo git checkout 01-Local-setup-Dev
```
#### Download the Dependencies
```
npm install
```
##### if you get Permission Issue then 

```
sudo chown -R ec2-user:ec2-user /home/ec2-user/NodeJS-2-tier-UMS-App
```

#### Start the Application
```
node server.js
```
#### Access Your Application in Browser
```
http://<Your-AWS-Public-IP>:3000
```
