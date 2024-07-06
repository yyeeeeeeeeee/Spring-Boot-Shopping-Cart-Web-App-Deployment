# Spring-Boot-Shopping-Cart-Web-App-Deployment


install java on jenkins ec2 
--sudo apt update
-- sudo pat install openjdk-17-jre-headless -y

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

start jenkins 
-- sudo systemctl enable jenkins
-- sudo systemctl start jenkins
-- sudo systemctl status jenkins
The Ultimate End-End CI/CD Project
Phase-1
Create 3 EC2 Instances with 25GB RAM and choose t2.medium

file:///home/raveen/Pictures/Screenshot%20from%202024-07-06%2011-57-48.png![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/d67e8e6a-6bde-4013-afd2-3973457947b6)

