# Spring-Boot-Shopping-Cart-Web-App-Deployment

## Phase-1

Create 3 EC2 Instances with 20GB storage, 4GB RAM and choose t2.medium

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/d67e8e6a-6bde-4013-afd2-3973457947b6)

When you create the instances, please edit the security group to allow inbound and outbound traffic.

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/df26d93a-0c09-4552-b27a-8cbf286085a8)

### Install Docker on All 3 VMs

#### Step-by-Step Installation

#### 1. Install prerequisite packages: 
   
```bash
   sudo apt-get update 
   sudo apt-get install ca-certificates curl
```

#### 2. Download and add Docker's official GPG key:

```bash
   sudo install -m 0755 -d /etc/apt/keyrings 
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc 
   sudo chmod a+r /etc/apt/keyrings/docker.asc
```

#### 3. Add Docker repository to Apt sources:

```bash
   echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

   sudo apt-get update
```

#### 4. Update package index:
  
```bash 
   sudo apt-get update
```

#### 5. Install Docker packages:
   
```bash
   sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

#### 6. Grant permission to Docker socket (optional, for convenience):

```bash
   sudo chmod 666 /var/run/docker.sock
```

By following these steps, you should have successfully installed Docker on your Ubuntu system. You can now start using Docker to containerize and manage your applications.

Follow this official document if you find any errors: Link: https://docs.docker.com/engine/install/ubuntu/

### Setting Up Jenkins on Ubuntu

#### Step-by-Step Installation

#### 1. Update the system:

```bash
   sudo apt-get update
   sudo apt-get upgrade -y
```

#### 2. Install Java (Jenkins requires Java):

```bash
   sudo apt install -y fontconfig openjdk-17-jre-headless -y
```

#### 3. Add Jenkins repository key:

```bash
   sudo wget -O /usr/share/keyrings/Jenkins-keyring.asc \
   https://pkg.Jenkins.io/debian-stable/Jenkins.io-2023.key
```

#### 4. Add Jenkins repository:

```bash
   echo \
      "deb [signed-by=/usr/share/keyrings/Jenkins-keyring.asc] \
      https://pkg.Jenkins.io/debian-stable binary/" | \
      sudo tee /etc/apt/sources.list.d/Jenkins.list > /dev/null
```

#### 5. Update the package index:

```bash
   sudo apt-get update
```

#### 6. Install Jenkins:

```bash
   sudo apt-get install -y Jenkins
```

#### 7. Start and enable Jenkins:

```bash
   sudo systemctl start Jenkins
   sudo systemctl enable Jenkins
```

#### 8. Access Jenkins:

   - Open a web browser and go to http://your_server_ip_or_domain:8080.
   - You will see a page asking for the initial admin password. Retrieve it using: sudo cat /var/lib/Jenkins/secrets/initialAdminPassword
   - Enter the password, install suggested plugins, and create your firstadmin user. or follow this official document link: https://www.Jenkins.io/doc/book/installing/linux/#debianubuntu


### SonarQube Setup:

-ssh into sonarqube ec2 instance

-run -> docker run -d –name sonar -p 9000:9000 sonarqube:lts-comminity

-access using <public_ip:9000>

username: admin
password:admin

### Nexus Setup:

-ssh into nexus ec2 instance

-run -> docker run -d –name nexus -p 8081:8081 sonatype/nexus3 

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
15. Pipeline Maven integration Plugin

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

### Create the pipeline in Jenkins.

Now write the Jenkinsfile

```
pipeline {
   agent any
   
   tools {
      jdk 'jdk17'
      maven 'maven3'
   }
   
environment {
   SCANNER_HOME = tool 'sonar-scanner'
}

stages {
   stage('Git Checkout') {
      steps {
         git branch: 'main', url: ‘https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-  Deployment.git’
            }
}

   stage('Compile') {
      steps {
         sh "mvn compile"
      }
   }
   
   stage('Unit Test') {
      steps {
         sh "mvn test -DskipTests=true"
      }
   }
   
   stage('SonarQube Analysis') {
      steps {
         withSonarQubeEnv('sonar') {
            sh '''$SCANNER_HOME/bin/sonar-scanner 
            -Dsonar.projectKey=Mission 
            -Dsonar.projectName=Mission 
            -Dsonar.java.binaries=.'''
         }
      }
   }

   stage('OWASP Dependency Check') {
      steps {
         dependencyCheck additionalarguments: ' --scan ./', odcInstallation: 'dependency-check' 
         dependencyCheckPublisher pattern: '**/dependency-check-report.xml' 
         }
      }
   }
stage('Trivy Scan File System') {
steps {
sh "trivy fs --format table -o trivy-fs-report.html ."
}
}

   stage('Build') {
      steps {
         sh "mvn package -DskipTests=true"
      }
   }

   stage('Deploy Artifacts To Nexus') {
      steps {
         withMaven(globalMavenSettingsConfig: 'maven-setting', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
            sh "mvn deploy -DskipTests=true"
         }
      }
   }
stage('Build & Tag Docker Image') {
steps {
script {
withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
sh "docker build -t bijan9438/monitor:latest ."
}
}
}
}
stage('Trivy Scan Image') {
steps {
sh "trivy image --format table -o trivy-image-report.html bijan9438/monitor:latest"
}
}
stage('Publish Docker Image') {
steps {
script {
withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
sh "docker push bijan9438/monitor:latest"
}
}
}
}
stage('Deploy to EKS') {
steps {
withKubeConfig(caCertificate: '', clusterName: 'my-eks22', contextName: '',
credentialsId: 'k8-token', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl:
'https://FE0E7FFC80B64E124F6F3EA8EDA2FE7E.sk1.ap-south-1.eks.amazonaws.com') {
sh "kubectl apply -f ds.yml -n webapps"
sleep 60
}
}
}
stage('Verify deployment') {
steps {
withKubeConfig(caCertificate: '', clusterName: 'my-eks22', contextName: '',
credentialsId: 'k8-token', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl:
'https://FE0E7FFC80B64E124F6F3EA8EDA2FE7E.sk1.ap-south-1.eks.amazonaws.com') {
sh "kubectl get pods -n webapps"
sh "kubectl get svc -n webapps"
}
}
}
}
post {
always {
script {
def jobName = env.JOB_NAME
def buildNumber = env.BUILD_NUMBER
def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 'red'
def body = """
<html>
<body>
<div style="border: 4px solid ${bannerColor}; padding: 10px;">
<h2>${jobName} - Build ${buildNumber}</h2>
<div style="background-color: ${bannerColor}; padding: 10px;">
<h3 style="color: white;">Pipeline Status: ${pipelineStatus.toUpperCase()}</h3>
</div>
<p>Check the <a href="${BUILD_URL}">console output</a>.</p>
</div>
</body>
</html>
"""
emailext (
subject: "${jobName} - Build ${buildNumber} - ${pipelineStatus.toUpperCase()}",
body: body,
to: 'place-your-email@gmail.com',
from: 'jenkins@example.com',
replyTo: 'jenkins@example.com',
mimeType: 'text/html',
attachmentsPattern: 'trivy-image-report.html'
)
}
}
}
}

```

### Setup K8 Cluster
=================================
Create one master and two worker nodes (3 EC2 instances with 20GB storage, t2.medium, Ubuntu image)

#### 1. Update System Packages [On Master & Worker Nodes]

```bash
sudo apt-get update
```

#### 2. Install Docker[On Master & Worker Nodes]

```bash
sudo apt install docker.io -y
sudo chmod 666 /var/run/docker.sock
```

#### 3. Install Required Dependencies for Kubernetes[On Master & Worker Nodes]

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
sudo mkdir -p -m 755 /etc/apt/keyrings
```

#### 4. Add Kubernetes Repository and GPG Key[On Master & Worker Nodes]

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

#### 5. Update Package List[On Master & Worker Node]

```bash
sudo apt update
```

#### 6. Install Kubernetes Components[On Master & Worker Nodes]

```bash
sudo apt install -y kubeadm=1.28.1-1.1 kubelet=1.28.1-1.1 kubectl=1.28.1-1.1
```

#### 7. Initialize Kubernetes Master Node [On MasterNode]

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

After running the above command, you will receive below below-highlighted. Make sure to run that on the worker nodes so as to join them with the master node.

![image](https://github.com/RavDas/Spring-Boot-Shopping-Cart-Web-App-Deployment/assets/86109995/8db6b226-294f-4790-bf93-2ecc9fae3bec)

#### 8. Configure Kubernetes Cluster [On MasterNode]

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### 9. Deploy Networking Solution (Calico) [On MasterNode]

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

#### 10. Deploy Ingress Controller (NGINX) [On MasterNode]

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml
```
