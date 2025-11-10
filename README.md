
# CI/CD Pipeline Setup on AWS EC2 using Jenkins, SonarQube, Nexus & Nginx

This project demonstrates a complete end-to-end CI/CD pipeline implementation using Jenkins, SonarQube, Nexus, and Nginx — each hosted on a dedicated AWS EC2 instance.
The pipeline automates build, test, code analysis, artifact storage, and deployment across multiple servers.

### Architecture Overview

##### Tool	Purpose	Port	Instance Type
##### Jenkins	CI/CD Orchestration	8080	t2.medium
##### SonarQube	Code Quality Analysis	9000	t2.large
##### Nexus	Artifact Repository	8081	t2.large
##### Nginx	Web Server (Deployment)	80	t2.micro

## Step 1: Create EC2 Instances

Create four EC2 instances for Jenkins, SonarQube, Nexus, and Nginx.

Configuration:

AMI: Ubuntu 22.04 LTS

Key Pair: Select existing or create new

Security Group (Inbound Rules):

Jenkins → 8080

SonarQube → 9000

Nexus → 8081

Nginx → 80

<img width="1919" height="781" alt="Image" src="https://github.com/user-attachments/assets/df3db001-021d-4f03-8975-5c001d008f9b" />

## Step 2: Connect to EC2 Instances via SSH

Use the SSH command from your AWS console or terminal.

### Connecting via SSH

<img width="1919" height="871" alt="Image" src="https://github.com/user-attachments/assets/e4fd4f30-16d6-4a2f-aa7e-b49d7c934929" />

## Step 3: Install Java & Jenkins on Jenkins Server

sudo apt update

sudo apt install fontconfig openjdk-21-jre -y

java -version

### Java Installation

<img width="1473" height="759" alt="Image" src="https://github.com/user-attachments/assets/1033472e-08ea-4876-ba01-480b6c4e7549" />

### Install Jenkins:

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update

sudo apt install jenkins -y

### Jenkins Installation

<img width="1466" height="749" alt="Image" src="https://github.com/user-attachments/assets/bcb00fe3-4512-473e-b1c9-b55965e1a91c" />

## Step 4: Verify Jenkins Port (8080)

sudo apt install net-tools -y

sudo netstat -ntpl

### Port 8080 Verification

<img width="1466" height="271" alt="Image" src="https://github.com/user-attachments/assets/1422f727-13f9-4c0a-bd8e-2a19735b5a31" />
 
## Step 5: Access Jenkins Dashboard

Open Jenkins in your browser:

http://<your-public-ip>:8080

### Initial Jenkins Setup

<img width="1232" height="868" alt="Image" src="https://github.com/user-attachments/assets/ee78f044-606b-415a-ab11-a5987c172433" />

## Step 6: Retrieve Jenkins Admin Password

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

### Admin Password Retrieval

<img width="1474" height="412" alt="Image" src="https://github.com/user-attachments/assets/b2b9d3c1-5c6b-43a7-bc0e-bbae78057b21" />

## Step 7: Install Suggested Plugins

Select “Install Suggested Plugins”.

<img width="1232" height="656" alt="Image" src="https://github.com/user-attachments/assets/0c063026-ee06-4bab-a3b6-d25980c32918" /> <img width="1241" height="847" alt="Image" src="https://github.com/user-attachments/assets/4716c3e1-19bb-4e0a-88e3-150b3e2c64bd" />

## Step 8: Create Admin Credentials

<img width="1917" height="942" alt="Image" src="https://github.com/user-attachments/assets/ec4c139b-340d-4666-8bd8-4eae5ecb3134" />

### Confirm the instance configuration URL and continue:

<img width="1245" height="850" alt="Image" src="https://github.com/user-attachments/assets/1424b279-31a4-46b9-8daf-08a9943d3761" />
 
## Step 9: Explore Jenkins Dashboard

<img width="1919" height="912" alt="bf3ae45ef27608e1f75b54d4905bf351_Screenshot%202025-11-07%20100210" src="https://github.com/user-attachments/assets/20c8b823-6ca3-4b2b-8aa9-ca7a7566f3dd" />

## Step 10: Setup SonarQube Server

### Connect via SSH:

<img width="1462" height="766" alt="Image" src="https://github.com/user-attachments/assets/de59dad4-17e5-47e0-9b7f-bb646eefba35" />

### Install Java:

sudo apt update

sudo apt install fontconfig openjdk-21-jre -y

java -version

### Java Verification

<img width="1464" height="758" alt="Image" src="https://github.com/user-attachments/assets/570f8577-a411-4356-9871-d4aebe7ccbb8" />

### Install SonarQube

cd /opt

sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.6.0.92116.zip

sudo unzip sonarqube-10.6.0.92116.zip

sudo mv sonarqube-10.6.0.92116 /opt/sonarqube

sudo useradd -r -s /bin/false sonar

sudo chown -R sonar:sonar /opt/sonarqube

### SonarQube Setup

<img width="1462" height="748" alt="Image" src="https://github.com/user-attachments/assets/855a85c0-fce4-4fd1-8cf8-cd1edaa03bf7" />

### Access SonarQube in your browser:

http://<SONAR_IP>:9000

Default login: admin / admin

<img width="1907" height="902" alt="Image" src="https://github.com/user-attachments/assets/40d5898a-8ef1-4a91-af35-c756c7983623" />

### Generate Token → My Account → Security → Generate Token

<img width="1279" height="615" alt="Image" src="https://github.com/user-attachments/assets/c7841d75-f180-45a7-aaaa-349c7b1afa7e" />

## Step 11: Setup Nexus Repository

### Connect to Nexus EC2 via SSH:

<img width="1919" height="824" alt="Image" src="https://github.com/user-attachments/assets/1f0b65e7-bdda-4460-a393-d9faf48b0520" />

### Install Java 17:

sudo apt update

sudo apt install -y openjdk-17-jdk wget tar

#### Download & extract Nexus:

cd /opt

wget https://download.sonatype.com/nexus/3/nexus-3.85.0-03-linux-x86_64.tar.gz

sudo tar -xzf nexus-3.85.0-03-linux-x86_64.tar.gz


### Run Nexus

<img width="1466" height="751" alt="Image" src="https://github.com/user-attachments/assets/5f05a335-dfe3-4c7f-8ee1-b6c374c7c92a" />

### Access Nexus:

http://<NEXUS_IP>:8081

Initial password path: /opt/sonatype-work/nexus3/admin.password

<img width="1914" height="872" alt="Image" src="https://github.com/user-attachments/assets/0a8ee24d-4631-4b10-9470-09e22be3aa00" />

## Step 12: Setup Nginx Server

<img width="1462" height="741" alt="Image" src="https://github.com/user-attachments/assets/ef01a399-0749-4f79-881f-d5fe422a548c" />

### Install Nginx:

sudo apt update

sudo apt install nginx -y

sudo systemctl start nginx

sudo systemctl status nginx

#### Nginx Running

<img width="1910" height="754" alt="Image" src="https://github.com/user-attachments/assets/8c55fa27-d3dd-4ffd-af10-406a97594185" />
 
## Step 13: Configure Build Node

#### Install Node.js & Angular CLI:

curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -

sudo apt install -y nodejs

sudo npm install -g @angular/cli

###  Node.js Installation

<img width="1473" height="749" alt="Image" src="https://github.com/user-attachments/assets/e6a96692-508e-44d5-9f18-48dc28bc594d" />

### Configure Sonar Scanner:

cd conf

sudo nano sonar-scanner.properties

Add:

sonar.host.url=http://<SONAR_IP>:9000

sonar.login=<TOKEN>


### Sonar Configuration

<img width="1463" height="757" alt="Image" src="https://github.com/user-attachments/assets/988aae0f-8ac5-42bc-9040-d39354503a39" />

### Sonar Configuration

sudo nano sonar-project.properties


<img width="1471" height="743" alt="81a050f266a3f26358ccffd0508e5e2c_Screenshot%202025-11-09%20225625" src="https://github.com/user-attachments/assets/54294730-a799-4d56-9723-20a5c3a0be07" />

## Step 14: Generate SSH Keys

sudo su jenkins

ssh-keygen

<img width="1465" height="752" alt="c5740de002fbaa8c6e85b168dcf3326a_Screenshot%202025-11-07%20125214" src="https://github.com/user-attachments/assets/d2eea691-cf1a-4f8a-9039-24165f3da084" />

Copy the public key to agent servers (~/.ssh/authorized_keys).

<img width="1487" height="406" alt="df938fa07425f7c29005eba70faac014_Screenshot%202025-11-07%20125321" src="https://github.com/user-attachments/assets/148785f7-98e3-4bb2-b4db-40ef066a2606" />

<img width="1474" height="752" alt="Image" src="https://github.com/user-attachments/assets/1924b290-31c6-4c34-bb51-703e67637dcf" />

Verify with SSH connection between Jenkins and agents.

By: ssh <username@IP address>

<img width="1465" height="759" alt="e7f3cb032f78ccd22b7cd259468477ee_Screenshot%202025-11-07%20125615" src="https://github.com/user-attachments/assets/697e877d-75a3-4059-b7b1-56bb6589bb03" />

<img width="1486" height="760" alt="2aa8c1f5c6300544de2f0d706ed70bbb_Screenshot%202025-11-07%20125802" src="https://github.com/user-attachments/assets/0ca258d0-0194-4d86-ab6e-e688a5a70213" />

<img width="1467" height="746" alt="b5b592ca8c4af884535072d624751a15_Screenshot%202025-11-07%20125819" src="https://github.com/user-attachments/assets/b408f447-4cad-498a-be05-a8ac62922e32" />

## Step 15: Configure Jenkins

Install Plugins:

Git Plugin

GitHub Integration

Pipeline

NodeJS

SonarQube Scanner

SSH Agents

<img width="1919" height="892" alt="Image" src="https://github.com/user-attachments/assets/0b45e5ed-840e-4ff0-ab61-33f0935792dc" />
 
## Step 16: Jenkins Global Tool Configuration

JDK 17

NodeJS 24

<img width="1919" height="900" alt="Image" src="https://github.com/user-attachments/assets/a1dc3f3b-9658-41cf-8fde-a57fab162c79" /> <img width="1879" height="901" alt="Image" src="https://github.com/user-attachments/assets/d3fb0ce3-c3a7-491f-8b1a-262d9446e14a" />
 
## Step 17: Add Credentials in Jenkins

Manage Jenkins → Credentials → System → Global → Add Credentials

Type	ID	Purpose

SSH Username with private key	ubuntu	Connect Jenkins to EC2 agents

Username & Password	nexus-creds	Access Nexus repository

Secret text is to connect with sonar

#### SSH Username with private key

Kind: SSH Username with private key

ID: ubuntu

Username: ubuntu

Private key: Enter directly → from ssh key-gen paste the private ID

Description: SSH key for ubuntu user on agents

#### Nexus credentials

Kind: Username with password

ID: nexus

Username: admin

Password: admin123 

<img width="1919" height="808" alt="Image" src="https://github.com/user-attachments/assets/07193b3a-93a4-4ca3-89f7-0f2827cf7e95" />

## Step 18: Configure SonarQube Server in Jenkins

Manage Jenkins → Configure System → SonarQube servers

Name: SonarQube

Server URL: http://SONAR_IP:9000

Server authentication token: paste token generated in Sonar UI

Save

<img width="1919" height="898" alt="Image" src="https://github.com/user-attachments/assets/d9936fb0-ef3a-4109-9a96-c926be7d0f70" />
 
## Step 19: Create Jenkins Nodes

#### Manage Jenkins → Manage Nodes and Clouds → New Node

#### Create SonarNode

Name: Sonar

Type: Permanent Agent

Remote root directory: /home/ubuntu/jenkins

Labels: Sonar

Launch method: Launch agents via SSH

Host: Sonar VM IP

Credentials: ssh-ubuntu

Test connection → Save

Add Sonar, Nexus, and Nginx nodes using SSH launch method.

Verify both nodes show Online.

<img width="1919" height="896" alt="Image" src="https://github.com/user-attachments/assets/4345e172-acbb-4b11-a9ab-a49872166744" />

<img width="1919" height="886" alt="Image" src="https://github.com/user-attachments/assets/3044f604-d8c4-4c58-860d-03589ddc889a" /> 

<img width="1918" height="899" alt="Image" src="https://github.com/user-attachments/assets/8619b21b-81d2-4ed1-8e9f-fa8aff4d91a9" />

<img width="1919" height="908" alt="ac2c71eedabcc15afdc0126e3ed169db_Screenshot%202025-11-08%20172810" src="https://github.com/user-attachments/assets/d21ab74d-5168-4a17-8d6f-4e70114788d2" />


## Step 20: Create Jenkins Pipeline

<img width="1919" height="909" alt="Image" src="https://github.com/user-attachments/assets/e06caa95-920e-4385-b762-a70a0d014def" />

<img width="1919" height="914" alt="1126183ab59e6140224d4d1a6ee72f83_Screenshot%202025-11-08%20173255" src="https://github.com/user-attachments/assets/dfb81317-7671-44b6-a6a2-98e311e7a8dc" />

Run the pipeline — it should pass all stages successfully.

<img width="1919" height="849" alt="Image" src="https://github.com/user-attachments/assets/417b748a-6a8d-4a09-a017-6de7f6f1f39a" />
 
## Step 21: Verify Build Results

#### SonarQube - Code Analysis

<img width="1919" height="851" alt="Image" src="https://github.com/user-attachments/assets/cb05f939-dbb4-4fac-8333-7ec70e24ebf6" />

#### Nexus - Artifact Storage

<img width="1919" height="910" alt="Image" src="https://github.com/user-attachments/assets/b460fb42-114f-42b1-b581-4be5ed38d7d0" />

#### Nginx - Deployment Output

<img width="1919" height="902" alt="Image" src="https://github.com/user-attachments/assets/7cb25a17-9dc6-494d-98d2-6f438a1ec5f7" />

### Final Outcome

This setup delivers a fully automated CI/CD pipeline:

Code pushed to GitHub triggers Jenkins.

SonarQube performs code quality analysis.

Nexus stores build artifacts.

Nginx deploys the final application automatically.
