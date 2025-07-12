# CI/CD-Pipeline-Using-DevOps-Tools
**Created a Complete CI/CD Pipeline from Development to Testing to Production using DevOps Tools**

You are hired as a DevOps engineer for Analytics Pvt Ltd. This company is a product based organization which uses Docker for their containerization needs within the company. The final product received a lot of traction in the first few weeks of launch. Now with the increasing demand, the organization needs to have a platform for automating deployment, scaling, and operations of application containers across clusters of hosts, As a DevOps engineer, you need implement a DevOps life cycle, such that all the requirements are implemented without any change in the Docker containers in the testing environment. 
Up until now, this organization used to follow a monolithic architecture with just 2 developers. 
The product is present on https://github.com/hshar/website.git 
Following are the specifications of life-cycle: 
1. Git workflow should be implemented. Since the company follows monolithic architecture of Development you need to take care of version control. The release should happen only on 25th of every month. 
2. Code build should be triggered once the commits are made in the master Branch. 
3. The code should be containerized with the help of the Docker file, The Dockerfile should be built every time if there is a push to Git-Hub. Create a custom Docker image using a Dockerfile. 
4. As per the requirement in the production server, you need to use the Kubernetes cluster and the containerized code from Docker hub should be deployed with 2 replicas. Create a NodePort service and configure the same for port 30008.  
5. Create a Jenkins pipeline script to accomplish the above task. 
6. For configuration management of the infrastructure, you need to deploy the configuration on the servers to install necessary software and configurations. 
7. Using Terraform accomplish the task of infrastructure creation in the AWS cloud provider. 
Architectural Advice Software’s to be installed on the respective machines using configuration management.
Worker1: Jenkins, Java. 
Worker2: Docker, Kubernetes. 
Worker3: Java, Docker, Kubernetes 
Worker4: Docker, Kubernetes

<img width="274" alt="image" src="https://github.com/Sivakami-vinoth/CICD-Pipeline-Using-DevOps-Tools/assets/125202974/fb65ca5a-201f-49f4-964c-b5b8c41ddbb3">

<img width="261" alt="image" src="https://github.com/Sivakami-vinoth/CICD-Pipeline-Using-DevOps-Tools/assets/125202974/4eb8015c-3d8b-47b5-a5e1-b6f62a9d694a">

______________________________________________________________________________________________________________________________________________________________

**Step:1 Create an EC2 Instances Machine-1 manually**

Machine-1: OS-> ubuntu, Instance type-> t2.medium

<img width="602" height="106" alt="image" src="https://github.com/user-attachments/assets/f17fc916-4db5-43fd-8b0d-f4a340f63da5" />

**Connect to the instance via mobaxterm**

<img width="602" height="258" alt="image" src="https://github.com/user-attachments/assets/cabfe2d6-c30c-4cd9-9333-4cc408a3bcd9" />

**Step:2 Intsall Terrform in Machine-1**

Open the link -> Install | Terraform | HashiCorp Developer

<img width="602" height="202" alt="image" src="https://github.com/user-attachments/assets/10780ed6-ac91-4cc1-ad76-c84ecc02ec32" />

**Copy and Run the commands**

<pre> 

wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update && sudo apt install terraform
</pre>

**Terraform Installed successfully**

<img width="407" height="53" alt="image" src="https://github.com/user-attachments/assets/dbe4417d-8276-4ff5-aaa9-4c35e522c5ac" />


**Step:3 Create main.tf file (To create machines kubernetes master, slave1 and slave2)**

Create the access keys -> open AWS console-> IAM
(Copy the access key and secret key)

<img width="602" height="196" alt="image" src="https://github.com/user-attachments/assets/5c84a227-9c30-4801-8baa-66590a0e8e2d" />

**Create main.tf file**

<img width="378" height="21" alt="image" src="https://github.com/user-attachments/assets/4e0a3a5d-eeb5-4229-bbe2-b201a271b4de" />


<pre>
provider "aws" {
 region = "us-east-1"
 access_key = "AKIAUYIFYE3ACB3UJFGI"
 secret_key = "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"
}
resource "aws_instance" "k8s-master" {
        ami = "ami-0261755bbcb8c4a84"
        instance_type = "t2.medium"
        key_name = "aws-keypair"
        tags = {
        Name = "Machine-2"
        }
}
resource "aws_instance" "k8s-slave1" {
        ami = "ami-0261755bbcb8c4a84"
        instance_type = "t2.micro"
        key_name = "aws-keypair"
        tags = {
        Name = "Machine-3"
        }
}
resource "aws_instance" "k8s-slave2" {
        ami = "ami-0261755bbcb8c4a84"
        instance_type = "t2.micro"
        key_name = "aws-keypair"
        tags = {
        Name = "Machine-4"
        }
}
</pre>

**Run the following commands from Machine-1**

**Run command -> terraform init**

<img width="601" height="153" alt="image" src="https://github.com/user-attachments/assets/400a5354-5d06-4e9b-95f5-74ca3b3bdc81" />

**Run ->terraform plan**

 <img width="602" height="86" alt="image" src="https://github.com/user-attachments/assets/b2b2cbc0-6959-47a1-821f-48345597e1cb" />

 <img width="602" height="165" alt="image" src="https://github.com/user-attachments/assets/a5f6ea1b-6029-4fb5-bffe-f01ab6b13a65" />

** Run ->terraform apply**
 
 <img width="602" height="55" alt="image" src="https://github.com/user-attachments/assets/374a3b6d-857a-457d-a578-abace2b03780" />

 <img width="602" height="262" alt="image" src="https://github.com/user-attachments/assets/2b057d58-7fa8-4e50-8add-69f20ccfbdfc" />
 

**Machines Kubernetes master(Machine-2), slave1(Machine-3) and slave3(Machine-4) created successfully**

 <img width="602" height="124" alt="image" src="https://github.com/user-attachments/assets/074f1f43-e889-4866-a7fb-8d288c5545b6" />

 <img width="602" height="106" alt="image" src="https://github.com/user-attachments/assets/7d0866dd-9fa0-4591-bb57-7212748ab949" />

 <img width="602" height="116" alt="image" src="https://github.com/user-attachments/assets/51253f78-2dfe-4cb2-847a-826e9f462928" />

 <img width="602" height="101" alt="image" src="https://github.com/user-attachments/assets/7e77067f-af14-4724-a9eb-3063b6b239e5" />


**Step:4 Intsall Ansible in Machine-1**

**Open the link-> Installing Ansible on specific operating systems — Ansible Documentation**

**Copy the commands and run one by one**

<pre>
<img width="602" height="206" alt="image" src="https://github.com/user-attachments/assets/819bd068-a18d-404f-832f-6d75cbfbe54e" />
</pre>

**Generate keypair**

**Run command->ssh-keygen**

<img width="602" height="372" alt="image" src="https://github.com/user-attachments/assets/9c785c50-2050-4e56-996e-1b112a46f106" />

**Run command -> cd .ssh and ls**

<img width="320" height="51" alt="image" src="https://github.com/user-attachments/assets/87582004-a235-4776-91d6-61bb9105b4c2" />

**Run command -> cat id_rsa.pub (copy the content)**

<img width="602" height="43" alt="image" src="https://github.com/user-attachments/assets/a1e901b3-5ce7-4866-9e58-bc361fb5b9e1" />

**Paste the content in the authorized_keys file from k8s-master, k8s-slave1 and k8s-slave2 Machines**

<img width="602" height="31" alt="image" src="https://github.com/user-attachments/assets/09f1e283-c5b9-454c-8abc-a7ffb0c7fb66" />

<img width="602" height="52" alt="image" src="https://github.com/user-attachments/assets/44e5a9fd-1038-4bec-9e96-e26c31765909" />

<img width="602" height="36" alt="image" src="https://github.com/user-attachments/assets/989dd354-aca1-4056-a56b-6a8e53d2bc0d" />

<img width="602" height="39" alt="image" src="https://github.com/user-attachments/assets/f2749791-34cf-446c-8037-94b22cf48db0" />

<img width="602" height="30" alt="image" src="https://github.com/user-attachments/assets/c6cca5fa-150e-464b-a0c8-e5be081bbfce" />

<img width="602" height="37" alt="image" src="https://github.com/user-attachments/assets/3415e216-8ab6-4ade-9cbc-5dcbc60a4e0f" />

**Update the ansible hosts**


<img width="578" height="55" alt="image" src="https://github.com/user-attachments/assets/ea31454c-7ada-4813-a97c-cc176548db7c" />

**Copy the private ip address of K8s-master, K8s-slave1 and K8s-slave2**


<img width="602" height="102" alt="image" src="https://github.com/user-attachments/assets/665883c4-f85b-4365-bff1-725f2d0843e6" />

**Run command -> ansible –m ping all**


<img width="563" height="359" alt="image" src="https://github.com/user-attachments/assets/371a8147-5f38-41d0-9a61-685e78845a94" />


**Step:5 Create script files to install java, docker, kubernetes and jenkins**

**Create file script1.sh to install java and jenkins im Machine-1**

**Open the link and copy the code->Linux (jenkins.io)**



<img width="602" height="198" alt="image" src="https://github.com/user-attachments/assets/29561762-8329-427d-b9da-b5aa4a6d0a1b" />


<img width="409" height="24" alt="image" src="https://github.com/user-attachments/assets/9f234462-989e-42f2-8711-5a07a74b7e0a" />

<pre>
sudo apt update
sudo apt install openjdk-11-jdk -y
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
</pre>


<img width="602" height="165" alt="image" src="https://github.com/user-attachments/assets/008931ac-ee42-4beb-b083-a949905d91a1" />

**Create script2.sh to install java, docker and kubernetes in K8s-master machine**


<img width="433" height="21" alt="image" src="https://github.com/user-attachments/assets/e3d8d457-babc-40ca-93ca-e48c65cc7276" />


<pre> 
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install docker.io -y
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl apt-transport-https ca-certificates software-properties-common
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo add-apt-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo swapoff -a
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
</pre>


<img width="602" height="188" alt="image" src="https://github.com/user-attachments/assets/0b5b8a92-7d68-4ab8-bbe5-7654b8839dd0" />

**Create script3.sh to install docker and kubernetes in K8s-slave1 and K8s-slave2**


<img width="401" height="31" alt="image" src="https://github.com/user-attachments/assets/e11adeeb-51de-48f2-83fb-42edab1cb76c" />

<pre> 
sudo apt update
sudo apt install docker.io -y
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl apt-transport-https ca-certificates software-properties-common
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo add-apt-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo swapoff -a
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
</pre>


<img width="602" height="143" alt="image" src="https://github.com/user-attachments/assets/052e7506-9fee-4f72-8646-8a75c76ac6be" />

**Step:6 Create playbook playbook1.yaml**


<img width="442" height="25" alt="image" src="https://github.com/user-attachments/assets/c4801830-1fe7-436b-bbd0-4590927d2053" />

<pre> 
---
- name: install Jenkins and Java on Machine-1
  become: true
  hosts: localhost
  tasks:
  - name: running script1
    script: script1.sh
- name: install Java docker and kubernetes on Machine-2
  become: true
  hosts: master
  tasks:
  - name: running script2
    script: script2.sh
- name: install docker and kubernetes on Machine-3 and machine-4
  become: true
  hosts: slaves
  tasks:
  - name: running script3
    script: script3.sh

</pre>

<img width="601" height="277" alt="image" src="https://github.com/user-attachments/assets/3e701c29-0dc4-4343-9050-14b036f3028b" />

**Run command-> ansible-playbook playbook1.yaml –syntax-check**

<img width="602" height="41" alt="image" src="https://github.com/user-attachments/assets/8f4917d1-ce74-4d9c-9e6a-db1ab25ffddc" />


**Run command-> ansible-playbook playbook1.yaml –check**

<img width="602" height="340" alt="image" src="https://github.com/user-attachments/assets/c4c0fa6b-6be4-4608-8b24-7f85cd94c357" />

**Run command-> ansible-playbook playbook1.yaml**

<img width="602" height="335" alt="image" src="https://github.com/user-attachments/assets/14c976e2-2c70-4862-b16f-ab96cd753e27" />


**Step:7 Initialize the kubeadm cluster in k8s-master machine**

**Run command -> kubeadm init**

<img width="602" height="238" alt="image" src="https://github.com/user-attachments/assets/acf888a6-de9f-4b5f-b9a4-ebc171f7eacf" />

**Copy the token and run in slave machines**

<img width="602" height="131" alt="image" src="https://github.com/user-attachments/assets/1a52331e-78b9-4a8a-b1dc-bbca611edec6" />

 
**Also run the below commnds**

<img width="602" height="141" alt="image" src="https://github.com/user-attachments/assets/3d449544-6068-4e66-81d6-57123395ede2" />

**Run the copied token in k8s-slave1**

<img width="602" height="131" alt="image" src="https://github.com/user-attachments/assets/2a1ea71e-89dc-489b-94d4-b40ff0efc3d8" />

**Run the copied token in k8s-slave2**

<img width="602" height="140" alt="image" src="https://github.com/user-attachments/assets/b2d8291c-d934-47d4-af82-de7a48184412" />

**All the three nodes configured successfully**

<img width="602" height="77" alt="image" src="https://github.com/user-attachments/assets/2e8f1368-9148-4e7f-90b7-5521b8aad29c" />


**Step:7 Configure Jenkins dashboard in Machine-1**

**Jenkins installed successfully**

<img width="601" height="189" alt="image" src="https://github.com/user-attachments/assets/59c7575f-c087-43ac-acc0-274ac0641557" />

**Run command ->sudo cat /var/lib/jenkins/secrets/initialAdminPassword and copy the password**

<img width="602" height="53" alt="image" src="https://github.com/user-attachments/assets/2e633586-98d9-4659-a1ec-d127a7a25a19" />

**Paste the password and click Continue**

<img width="602" height="292" alt="image" src="https://github.com/user-attachments/assets/e156c432-a4c7-49a7-b88f-106688d5d01e" />

**Click on Install suggested plugins**

<img width="602" height="309" alt="image" src="https://github.com/user-attachments/assets/c3d8d6a7-8cd7-4311-98e6-8d77325849c4" />

<img width="602" height="300" alt="image" src="https://github.com/user-attachments/assets/9cfcec56-f2e5-48ce-9a1a-2e0454db9a42" />

**Enter the required information to create a admin user and click on save and continue**

<img width="602" height="236" alt="image" src="https://github.com/user-attachments/assets/2a0acb59-d55b-4df6-b028-c97e8848c951" />

<img width="602" height="254" alt="image" src="https://github.com/user-attachments/assets/5ec9e440-0b00-478e-beff-2672f34097aa" />

<img width="602" height="171" alt="image" src="https://github.com/user-attachments/assets/ecb086f4-730c-4b18-8536-3449e6f82097" />

**Jenkins Dashboard Configured successfully**

<img width="602" height="172" alt="image" src="https://github.com/user-attachments/assets/5058f5e6-26be-42b7-bc0a-4f0597b70fbb" />


**Step:7 Add k8s-master as jenkins node**

**Open manage jenkins and click on Nodes**

<img width="602" height="149" alt="image" src="https://github.com/user-attachments/assets/825ecc80-c44a-4941-ad3a-3561027de3dc" />

**Click on New Node**

<img width="602" height="159" alt="image" src="https://github.com/user-attachments/assets/eb8c083f-2998-409f-8c00-6199a919d2e0" />

**Enter node name and select Permanent agent and Click Create**

<img width="602" height="194" alt="image" src="https://github.com/user-attachments/assets/0d694a1a-98f7-405c-9216-14c964a034a9" />

**Enter the Remote root dirctory(/home/ubuntu/jenkins)**

<img width="602" height="214" alt="image" src="https://github.com/user-attachments/assets/2c0f17d6-8ea4-4a33-86da-274a6ff8f5d5" />


**Select Launch method-> Launch agents via ssh**

**Host-> Copy the private IP of the jenkins master machine(Machine-1)**

**Click on Add to add Credentials**

<img width="602" height="237" alt="image" src="https://github.com/user-attachments/assets/e7d21345-3e80-4c7c-83d7-bd94f68957af" />

**Copy the id_rsa (private key)**

<img width="569" height="249" alt="image" src="https://github.com/user-attachments/assets/bcdb1451-398f-4be4-8ea2-9ed3a29edce4" />

**Kind-> SSH Username with private key**

**Username-> ubuntu**

**Private ket-> Enter directly(Paste the copied key)**

**Click on add**

<img width="602" height="214" alt="image" src="https://github.com/user-attachments/assets/083d0110-1cab-45cb-88a3-37623d667f8d" />

<img width="602" height="188" alt="image" src="https://github.com/user-attachments/assets/587b7599-19b9-4737-87a3-7b6446dbf36b" />

<img width="602" height="81" alt="image" src="https://github.com/user-attachments/assets/a55bbcfa-7451-4e01-9f4b-e54c3f147b5e" />

**Select the Credential created**

**Select Host key verification strategy-> Non verifying verification Strategy and Click Save**

<img width="602" height="179" alt="image" src="https://github.com/user-attachments/assets/b7ebac9f-e0e3-49bf-9416-b819439f4285" />

<img width="602" height="211" alt="image" src="https://github.com/user-attachments/assets/89568dd5-0385-4138-813d-82f8b3fdc39f" />

**Node Added successfully**

<img width="602" height="108" alt="image" src="https://github.com/user-attachments/assets/14870b58-0674-499c-87e7-097d49cb64a6" />


**Step:8 Add Jenkins credentials for Docker hub**

**Open manage Jenkins and Click on Credentials**

<img width="602" height="211" alt="image" src="https://github.com/user-attachments/assets/bd02ffa3-d5ab-46a7-ab6e-2735f53da1eb" />

**Click on Global**

<img width="602" height="189" alt="image" src="https://github.com/user-attachments/assets/0fd416d8-6876-499e-9a21-008de14c08db" />

**Click Add Credentials**

<img width="602" height="120" alt="image" src="https://github.com/user-attachments/assets/3471297d-6b41-4418-9ac4-6ddd5a16ff00" />

**Enter your Docker Hub account Username and Password**

<img width="602" height="264" alt="image" src="https://github.com/user-attachments/assets/2380c503-722e-44eb-a3ac-75cdca488283" />

**Credentials added successfully**

<img width="602" height="121" alt="image" src="https://github.com/user-attachments/assets/765c0903-53e9-4ce4-9936-c31c86e3e817" />


**Step:9 Fork the https://github.com/hshar/website.git to your local account**

**Open the link https://github.com/hshar/website.git**

**Click on Fork**

<img width="602" height="204" alt="image" src="https://github.com/user-attachments/assets/3c7b1381-1f21-47e3-b68d-977e0a71cf27" />

**Click on Create Fork**

<img width="602" height="424" alt="image" src="https://github.com/user-attachments/assets/490f3ec7-d211-409d-9b16-842e5e4c7069" />


**Step:10 Create a Dockerfile in the Repository ‘website’**

**Open the Github account and open Respository ‘website’**

**Click on Add File-> Create New File**

<img width="602" height="255" alt="image" src="https://github.com/user-attachments/assets/ff6b13b6-a392-4412-a8db-8a67a26996ca" />

<img width="602" height="128" alt="image" src="https://github.com/user-attachments/assets/845b2fd6-f337-4f54-933e-0eabeb2493f3" />

 <img width="465" height="366" alt="image" src="https://github.com/user-attachments/assets/01990b88-b0e8-4f13-95ad-e4ce5f378276" />



**Step:11 Create a jenkins pipeline to print hello**

**Open the jenkins dashboard and Click on New item**

<img width="602" height="177" alt="image" src="https://github.com/user-attachments/assets/3117f3f9-9d86-419e-bebc-6caacb4c28ab" />

**Enter an item name and select Pipeline and Click ok**

<img width="602" height="363" alt="image" src="https://github.com/user-attachments/assets/d69f74fc-6312-436d-9a0f-01cdbb0d8aa3" />

<img width="602" height="363" alt="image" src="https://github.com/user-attachments/assets/12108dc1-ad7b-4ab0-8f73-c62dfbfa2c33" />

**Move down to script and select try sample pipeline-> Hello world**

<img width="602" height="281" alt="image" src="https://github.com/user-attachments/assets/ad4e36c2-7eb9-42b6-9d58-938424fc8638" />

**Copy the Credential id created for dockerhub**

<img width="602" height="122" alt="image" src="https://github.com/user-attachments/assets/ba67b443-4d1d-4451-adbb-cc3ed905637b" />

**Make the below changes in the Script and save the changes**

<pre>
 pipeline {
    agent none
    environment{
        DOCKERHUB_CREDENTIALS= credentials('c108847f-fa55-4d24-b84e-f4e1acf4c1a5')
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
</pre>


<img width="602" height="130" alt="image" src="https://github.com/user-attachments/assets/1163d9fc-4eca-4efc-9e86-32c965678b21" />

**Click on Build Now**


<img width="537" height="214" alt="image" src="https://github.com/user-attachments/assets/3f380ade-dd2f-45f1-b420-0606fd5c98ef" />



**Step:12 update the jenkins pipeline to copy the files from github to the K8s-master machine**

**Copy the url of the github respository ‘website’**


<img width="602" height="218" alt="image" src="https://github.com/user-attachments/assets/522e41c5-1536-429e-9ec4-64fbf97ea5c0" />

**Make the below changes to the pipleline script and save it**

<pre>
     stage('Git') {
            agent {
                label 'K8s-master'     
            }
            steps {
                git 'https://github.com/Sivakami-vinoth/website.git'
            }
        }
</pre>



<img width="602" height="272" alt="image" src="https://github.com/user-attachments/assets/c0d9e758-109b-4e9d-9eda-fda3792d4647" />

**Click on Build now**


<img width="601" height="295" alt="image" src="https://github.com/user-attachments/assets/26c20eae-4865-4272-9449-485f0c6c0ee7" />

**Now files are copied from git hub respository to k8s-master machine**


<img width="524" height="255" alt="image" src="https://github.com/user-attachments/assets/9bda863c-e1f9-4708-9c8b-2710f43748aa" />


**Step:13 update the jenkins pipeline to build and push the images to DockerHub**

**Update the pipeline script and save it**

<pre>
 stage('Docker') {
            agent {
                label 'K8s-master'     
            }
            steps {
                sh 'sudo docker build /home/ubuntu/jenkins/workspace/mypipeline -t sivakamivinoth/project2'
                sh 'sudo echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'sudo docker push sivakamivinoth/project2'
            }
        }
</pre>



<img width="602" height="241" alt="image" src="https://github.com/user-attachments/assets/6bd9a41f-860d-4915-b37c-2c2a50dd8586" />


<img width="602" height="332" alt="image" src="https://github.com/user-attachments/assets/934766fe-7794-44dd-8b93-7021dc38eae9" />


**Image have been pushed to the dockerhub account**


<img width="602" height="169" alt="image" src="https://github.com/user-attachments/assets/bce65979-ce1c-43c9-9eaf-135a58c90795" />


<img width="602" height="211" alt="image" src="https://github.com/user-attachments/assets/1e498087-761f-40d9-b8b6-082ae0005da0" />



**Step:14 update the jenkins pipeline to create kubernetes deployment and service**

**Create a new file deployment.yaml in the github repository**


<img width="602" height="260" alt="image" src="https://github.com/user-attachments/assets/9b1dce9e-0ad9-4c7d-8e7a-eafb9e13a941" />

<pre>
 apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: sivakamivinoth/project2:latest
        ports:
        - containerPort: 80
</pre>



<img width="602" height="264" alt="image" src="https://github.com/user-attachments/assets/df583461-1b3b-4702-b360-1fd9ca2672fb" />

**Click on Commit changes**


<img width="475" height="371" alt="image" src="https://github.com/user-attachments/assets/57eb3518-357e-4322-a5b4-2944b3c9a07f" />

**File deployment.yaml created successfully**


<img width="602" height="194" alt="image" src="https://github.com/user-attachments/assets/c61c9787-cb77-4336-8a6c-0d8acbdfed39" />

**Create a new file service.yaml**

<pre>
 apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
</pre>


<img width="602" height="211" alt="image" src="https://github.com/user-attachments/assets/be6bea72-dc70-48cf-839f-571bf9254a04" />

**Click on Commit Changes**


<img width="471" height="246" alt="image" src="https://github.com/user-attachments/assets/688e74f0-836f-478d-ae4d-f1a868e2ae50" />

**File service.yaml file created successfully**


<img width="602" height="265" alt="image" src="https://github.com/user-attachments/assets/f7a2496d-e01f-4418-8ade-2e825c74d023" />

**Update the pipeling script with below code**

<pre>
 stage('Kubernetes') {
            agent {
                label 'K8s-master'     
            }
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
</pre>


<img width="602" height="282" alt="image" src="https://github.com/user-attachments/assets/152623ce-6695-42f1-9170-f5b6a8e0bd7d" />

**Click on Build now**


<img width="602" height="334" alt="image" src="https://github.com/user-attachments/assets/64c58fce-63ba-43af-bb1c-e92bf1b1a610" />

**Application deployed on Kubernetes slave1 and slave2 successfully**


<img width="602" height="234" alt="image" src="https://github.com/user-attachments/assets/3a5f9524-f216-4078-a5eb-ee427049ae02" />


<img width="602" height="313" alt="image" src="https://github.com/user-attachments/assets/c16ac3d1-e5dd-43b8-948c-a310b034cf4e" />



**Step:15 Make changes to the pipeline to get triggered once any changes made in the master branch**

**Open the pipeline configuration and enable “GitHub hook trigger for GITScm polling and save it**


<img width="602" height="287" alt="image" src="https://github.com/user-attachments/assets/cbf3285c-d4a8-4206-884e-71f69582caf7" />


**Open github settings  and create Webhook**


<img width="602" height="203" alt="image" src="https://github.com/user-attachments/assets/c0ada491-0ea0-4d4e-a2c2-d7d1cad80509" />


<img width="602" height="376" alt="image" src="https://github.com/user-attachments/assets/dd4c2704-88f9-4dea-835b-a991c4f2b0e4" />


**Webhook created successfully**


<img width="602" height="119" alt="image" src="https://github.com/user-attachments/assets/ffcca7e6-8e9f-4884-b933-4811588f9423" />


**Update the pipeling script as below**

<pre>
 steps {
                sh 'kubectl delete deploy nginx-deployment'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
</pre>


<img width="602" height="264" alt="image" src="https://github.com/user-attachments/assets/69f57952-e17c-4358-9f3f-5572defad1d7" />


**Modify the index.html file in Master branch and commit changes**


<img width="602" height="161" alt="image" src="https://github.com/user-attachments/assets/368f07be-efa2-4e94-8e15-e3a59be08db6" />


<img width="371" height="395" alt="image" src="https://github.com/user-attachments/assets/791adc4f-6a85-4b15-9c92-3d8def877a76" />


**Pipeline triggered automatically**

<img width="602" height="286" alt="image" src="https://github.com/user-attachments/assets/94d37c8f-d2bb-4e41-8228-11c621362dab" />


**Kubernetes Slaves got updated with the new changes**


<img width="602" height="344" alt="image" src="https://github.com/user-attachments/assets/b38674c2-93bc-4dae-86d8-56c3d4bab54d" />


<img width="602" height="452" alt="image" src="https://github.com/user-attachments/assets/ed99cb2a-c2c1-4d64-9f78-ebeecde6abaf" />
























































































 
 
 












