# Spring-Boot-Shopping-Cart-Web-App-Deployment

## Phase-1

Create 3 EC2 Instances with 20GB RAM and choose t2.medium

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/d67e8e6a-6bde-4013-afd2-3973457947b6)

When you create the instances, please edit the security group to allow inbound and outbound traffic.

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/df26d93a-0c09-4552-b27a-8cbf286085a8)

### Install Docker on All 3 VMs

Step-by-Step Installation

1. Install prerequisite packages:
sudo apt-get update
sudo apt-get install ca-certificates curl

2. Download and add Docker's official GPG key:
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o
/etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

3. Add Docker repository to Apt sources:
echo "deb [arch=$(dpkg --print-architecture) signed
by=/etc/apt/keyrings/docker.asc]
https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo
"$VERSION_CODENAME") stable" | sudo tee
/etc/apt/sources.list.d/docker.list > /dev/null

4. Update package index:
sudo apt-get update

5. Install Docker packages:
sudo apt-get install docker-ce docker-ce-cli containerd.io -y

6. Grant permission to Docker socket (optional, for convenience):
sudo chmod 666 /var/run/docker.sock
By following these steps, you should have successfully installed Docker on your Ubuntu system. You can now start using Docker to containerize and manage your applications.

Follow this official document if you find any errors: Link: Install Docker Engine on Ubuntu | Docker Docs

### Setting Up Jenkins on Ubuntu

Step-by-Step Installation

1. Update the system:
sudo apt-get update
sudo apt-get upgrade -y

2. Install Java (Jenkins requires Java):
sudo apt install -y fontconfig openjdk-17-jre-headless -y

3. Add Jenkins repository key:
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc
https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

4. Add Jenkins repository:
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]
https://pkg.jenkins.io/debian-stable binary/" | sudo tee
/etc/apt/sources.list.d/jenkins.list > /dev/null

5. Update the package index:
sudo apt-get update

6. Install Jenkins:
sudo apt-get install -y jenkins

7. Start and enable Jenkins:
sudo systemctl start jenkins
sudo systemctl enable jenkins

8. Access Jenkins:
- Open a web browser and go to http://your_server_ip_or_domain:8080.
- You will see a page asking for the initial admin password. Retrieve it using: sudo cat /var/lib/jenkins/secrets/initialAdminPassword
- Enter the password, install suggested plugins, and create your firstadmin user. or follow this official document link: https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
