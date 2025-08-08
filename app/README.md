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
### Login to your mongoDB
```
mongosh
```
### Allow Remote Access
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
#### Restart mongoDB
```
sudo systemctl restart mongod
```

## Use mongo-compass in your Local Machine and try to access your MongoDB
```
mongodb://<your-AWS-Public-IP>:27017
```

# Setup Application Server
## Install Node
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
## Install Git
```
sudo yum install git -y
```
### To start this application first you can get the code using below url
#### Clone the Repo
```
git clone https://github.com/techizone-Small-Project-org/Nodejs-Static-Project.git
```
#### Switch to Local-setup Branch
```
cd Nodejs-Static-Project
git checkout Local-setup
```
#### Download the Dependencies
```
npm install
```
#### Start the Application
```
node app.js
```
#### Access Your Application in Browser
```
http://<Your-AWS-Public-IP>:9990/sapsecops
```
