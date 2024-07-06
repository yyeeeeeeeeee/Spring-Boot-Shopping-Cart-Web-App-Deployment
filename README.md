# Spring-Boot-Shopping-Cart-Web-App-Deployment

## Phase-1

Create 3 EC2 Instances with 20GB storage, 4GB RAM and choose t2.medium

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/d67e8e6a-6bde-4013-afd2-3973457947b6)

When you create the instances, please edit the security group to allow inbound and outbound traffic.

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/df26d93a-0c09-4552-b27a-8cbf286085a8)

### Install Docker on All 3 VMs

#### Step-by-Step Installation

#### 1. Install prerequisite packages: 
   
   sudo apt-get update \
   sudo apt-get install ca-certificates curl

#### 2. Download and add Docker's official GPG key:
   
   sudo install -m 0755 -d /etc/apt/keyrings \
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc \
   sudo chmod a+r /etc/apt/keyrings/docker.asc

#### 3. Add Docker repository to Apt sources:
   
   echo \
   &nbsp;&nbsp; "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu  \\ \
   &nbsp;&nbsp; $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |  \\ \
   &nbsp;&nbsp; sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  \\ \
   sudo apt-get update

#### 4. Update package index:
   
   sudo apt-get update

#### 5. Install Docker packages:
   
   sudo apt-get install docker-ce docker-ce-cli containerd.io -y

#### 6. Grant permission to Docker socket (optional, for convenience):

   sudo chmod 666 /var/run/docker.sock

By following these steps, you should have successfully installed Docker on your Ubuntu system. You can now start using Docker to containerize and manage your applications.

Follow this official document if you find any errors: Link: https://docs.docker.com/engine/install/ubuntu/

### Setting Up Jenkins on Ubuntu

#### Step-by-Step Installation

#### 1. Update the system:
   
   sudo apt-get update
   sudo apt-get upgrade -y

#### 2. Install Java (Jenkins requires Java):

   sudo apt install -y fontconfig openjdk-17-jre-headless -y

#### 3. Add Jenkins repository key:

   sudo wget -O /usr/share/keyrings/Jenkins-keyring.asc  \\ \
   &nbsp;&nbsp; https://pkg.Jenkins.io/debian-stable/Jenkins.io-2023.key

#### 4. Add Jenkins repository:

   echo  \\ \
   &nbsp;&nbsp; "deb [signed-by=/usr/share/keyrings/Jenkins-keyring.asc]   \\ \
   &nbsp;&nbsp; https://pkg.Jenkins.io/debian-stable binary/" |   \\ \
   &nbsp;&nbsp; sudo tee /etc/apt/sources.list.d/Jenkins.list > /dev/null

#### 5. Update the package index:

   sudo apt-get update

#### 6. Install Jenkins:

   sudo apt-get install -y Jenkins

#### 7. Start and enable Jenkins:

   sudo systemctl start Jenkins
   sudo systemctl enable Jenkins

#### 8. Access Jenkins:

   - Open a web browser and go to http://your_server_ip_or_domain:8080.
   - You will see a page asking for the initial admin password. Retrieve it using: sudo cat /var/lib/Jenkins/secrets/initialAdminPassword
   - Enter the password, install suggested plugins, and create your firstadmin user. or follow this official document link: https://www.Jenkins.io/doc/book/installing/linux/#debianubuntu


### SonarQube Setup:

-ssh into sonarqube ec2 instance

-docker run -d –name sonar -p 9000:9000 sonarqube:lts-comminity

-access using <publicip:9000>

username: admin
password:admin

### Nexus Setup:

-ssh into nexus ec2 instance

-docker run -d –name nexus -p 8081:8081 sonatype/nexus3 

-access using <public_ip:8081>

-go inside of the sonatype/nexus3 container: docker exec -it <container_id> /bin/bash 

-sign to nexus using the username: admin and password; the password is stored in ‘sonatype-work/nexus3/admin.password’. 
 
-view it using cat sonatype-work/nexus3/admin.password' command.

===================================
Install Plugins in Jenkins

1. Eclipse Temurin installer -> for jdk
2. Sonarqube scanner
3. Docker
4. Docker pipeline
5. ClodBees Docker Build and Publish
6. docker-build-step
7. OWASP Dependency-Check
8. Config file provider -> for Nexus
9. Nexus Artifact Uploader
10. Kubernetes
11. Kubernetes cli
12. Kubernetes credentials
13. Kubernetes clint api
14. Maven integration
15. Pipeline maven integration

Now we installed the tools and Now we need to configure them

Go to manage Jenkins -> Tools

1. Jdk -> name= jdk17 , install automatically from adoptium.net, version= jdk17 latest
2. Sonarqube scanner -> name= sonar-scanner, Install automatically
3. Maven -> name= maven3, version= 3.6.3
4. Dependency-Check installation -> name= dependency-check, version= dependency-check6.5.1
5. Docker -> name=docker, install automatically from docker.com


### Configure the SonarQube server in Jenkins

1. Firstly generate the token in SonarQube
   
Goto Administration -> security -> users -> update token -> name= sonar-token and
generate

2. Add the token to Jenkins

Goto manage Jenkins -> credentials -> global -> kind= secret text -> secret=<your-token>, id=sonar-token, description=sonar-token

3. Configure SonarQube servers in Jekins
Go to manage Jenkins -> system -> sonarqube server -> name=sonar, url=http://<public_ip>:9000, token=sonar-token


### Configure the Nexus server in Jenkins

Nexus authentication with Jenkins:

Go to manage Jenkins -> manage files -> add new config -> select Global Maven settings.xml,
id=maven-setting & click next

Go to content and add 
   &nbsp; i) servers with the id = maven-releases, username, and password of the Nexus instance. (one we are going to release for production environments)
   &nbsp; ii) servers with the id = maven-snapshots, username, and password of the Nexus instance. (one we are going to release for development environments)

Nexus Configuration:

Update your pom.xml file with your Nexus repositories.

Copy the maven-releases URL, and maven-snapshots URL from the Nexus Repository  and update in the pom.xml file in the code repository.




Add DockerHub credentials in Jenkins:

Goto manage Jenkins -> credentials -> kind=username and password, username=<your-
username>, password=<your-password>, id=docker-credentials, description=docker-credentials
