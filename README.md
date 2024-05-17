## Install Jenkins Server on EC2 Machine(Master Node) 

### Update Linux Packages

```
sudo yum -y update
```
###  Install Java 11 OR 17
```
sudo amazon-linux-extras install java-openjdk11 -y
```
```
sudo yum install java-17-amazon-corretto-headless -y
```

###   Install Jenkins Server 
```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```
```
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```
```
sudo yum install jenkins -y
```
```
sudo systemctl start jenkins
```
```
systemctl status jenkins
```
### Assign Root Privileges to Jenkins User

```
sudo vi /etc/sudoers  
```
```
jenkins ALL=(ALL) NOPASSWD: ALL
```

### Restart Jenkins server and enable it
```
sudo service jenkins restart
```
```
sudo systemctl enable jenkins
```
# = Create new Ec2 Instance and configure Slave Node =

### Update Linux Packages

```
sudo yum -y update
```
###  Install Java 11 OR 17
```
sudo amazon-linux-extras install java-openjdk11 -y
```
```
sudo yum install java-17-amazon-corretto-headless -y
```

### setup jenkins slave
```
sudo su
```
```
sudo useradd jenkins-slave1
```
```
passwd jenkins-slave1 # set password for user jenkins-slave1
```
```
sudo vi /etc/sudoers
```
```
jenkins-slave1 ALL=(ALL) NOPASSWD: ALL
```
```
sudo su - jenkins-slave1
```
```
ssh-keygen -t ed25519  or ssh-keygen
```
```
cd .ssh
```
```
cat id_ed25519.pub > authorized_keys
```
```
chmod 700 authorized_keys
```
### Configure Jenkins Master with Slave Node

#### Note:Execute the below commands on Master Node
```
sudo mkdir -p /var/lib/jenkins/.ssh
```
```
cd /var/lib/jenkins/
```
```
sudo chmod 777 .ssh
```
```
sudo ssh-keyscan -H  private-ipaddress-slave-node >>/var/lib/jenkins/.ssh/known_hosts
```
```
cd .ssh
```
```
sudo chown jenkins:jenkins known_hosts
```
```
sudo chmod 700 known_hosts
```
### Copy the key-pair from jenkins slave node

#### Note: Connect to your slave node
```
sudo su - jenkins-slave1
```
```
cd .ssh
```
```
cat id_ed25519
```
#### Create the credentials on the Jenkins Master using this keypair and username(jenkins-slave1)

#### Note: Allocate the below disk space for master and slave in Jenkins dashboard

![jenkins readme](https://github.com/naveen-uppala/Jenkins/assets/99358567/2de0de8a-11dd-4ad0-8c68-14cb1bfd1a7d)

