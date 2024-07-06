# Spring-Boot-Shopping-Cart-Web-App-Deployment

Phase-1

Create 3 EC2 Instances with 20GB RAM and choose t2.medium

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/d67e8e6a-6bde-4013-afd2-3973457947b6)

#Setting Up Jenkins on Ubuntu

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
