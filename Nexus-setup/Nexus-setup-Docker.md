## Step-1 : Setup Linux VM (Amazon Linux AMI)

1) Login into AWS Cloud account
2) Launch Linux VM using EC2 service   
     - AMI : Amazon Linux
     - Instance Type : t2.medium       
4) Connect to VM using MobaXterm

## Step-2 : Install Docker In Linux VM

```
sudo yum update -y 
sudo yum install docker -y
sudo service docker start
sudo usermod -aG docker ec2-user
exit
```
## Step-3 :  Verify docker installation
```
docker -v
```
## Step-4 : Run Nexus using docker image
```
docker run -d -p 8081:8081 --name nexus sonatype/nexus3

does two different things:

Inside the EC2 instance: Maps container port 8081 to EC2 host port 8081.
AWS Security Group: Controls whether traffic from outside the EC2 instance is allowed to reach that port.
Why open port 8081 in the Security Group?

Nexus listens on port 8081 by default.

When you run:

-p 8081:8081

you're telling Docker:

"Forward requests arriving at the EC2 instance on port 8081 to the Nexus container on port 8081."

However, AWS Security Groups act as a firewall. Even if Docker is listening on port 8081, external users cannot access it unless the Security Group allows inbound traffic on that port.

Without Security Group Rule
Browser
   |
   | http://EC2-Public-IP:8081
   |
AWS Security Group  ❌ Blocks Port 8081
   |
EC2 Instance
   |
Docker Container (Nexus)

Result: Connection timeout or site can't be reached.

With Security Group Rule
Browser
   |
   | http://EC2-Public-IP:8081
   |
AWS Security Group  ✅ Allows Port 8081
   |
EC2 Instance
   |
Docker Container (Nexus)

Result: Nexus UI opens successfully.

Typical Security Group Rule
Type	Protocol	Port	Source
Custom TCP	TCP	8081	Your IP or 0.0.0.0/0

For better security, allow only your public IP instead of 0.0.0.0/0 whenever possible.

In short: Docker exposes the port on the EC2 server, but the AWS Security Group must allow incoming traffic to that port from the internet.
```

## Step-5: Enable 8081 port number in Security Group Inbound Rules & Access Nexus Server
```
 http://public-ip:8081/
```
## Step-6: Get nexus passsword from Docker Container
```
 docker ps
```
```
 docker exec -it <container-id> /bin/bash
```
```
 cat /nexus-data/admin.password 
```

## Step-7: Login into Nexus Server & Reset pwd