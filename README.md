# CI/CD-Pipeline-Using-DevOps-Tools
**Created a Complete CI/CD Pipeline from Development to Testing to Production using DevOps Tools**

You are hired as a DevOps engineer for Analytics Pvt Ltd. This company is a product based organization which uses Docker for their containerization needs within the company. The final product received a lot of traction in the first few weeks of launch. Now with the increasing demand, the organization needs to have a platform for automating deployment, scaling, and operations of application containers across clusters of hosts, As a DevOps engineer, you need implement a DevOps life cycle, such that all the requirements are implemented without any change in the Docker containers in the testing environment. 
Up until now, this organization used to follow a monolithic architecture with just 2 developers. 
The product is present on https://github.com/hshar/website.git 
Following are the specifications of life-cycle: 
1. Git workflow should be implemented. Since the company follows monolithic architecture of Development you need to take care of version control. The release should happen only on 25th of every month. 
2. Code build should be triggered once the commits are made in the master Branch. 
3. The code should be containerized with the help of the Docker file, The Dockerfile should be built every time if there is a push to Git-Hub. Create a custom Docker image using a Dockerfile. 
4. As per the requirement in the production server, you need to use the Kubernetes cluster and the containerized code from Docker hub should be deployed with 2 replicas. Create a NodePort service and configure the same for port 30008. www.intellipaat.com www.intellipaat.com 
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

**Step:1 Create an EC2 Instances Machine-1 manually******

Machine-1: OS-> ubuntu, Instance type-> t2.medium

<img width="602" height="106" alt="image" src="https://github.com/user-attachments/assets/f17fc916-4db5-43fd-8b0d-f4a340f63da5" />

**Connect to the instance via mobaxterm******

<img width="602" height="258" alt="image" src="https://github.com/user-attachments/assets/cabfe2d6-c30c-4cd9-9333-4cc408a3bcd9" />

**Step:2 Intsall Terrform in Machine-1******

Open the link -> Install | Terraform | HashiCorp Developer

<img width="602" height="202" alt="image" src="https://github.com/user-attachments/assets/10780ed6-ac91-4cc1-ad76-c84ecc02ec32" />

**Copy and Run the commands******

wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update && sudo apt install terraform


Terraform Installed successfully

<img width="407" height="53" alt="image" src="https://github.com/user-attachments/assets/dbe4417d-8276-4ff5-aaa9-4c35e522c5ac" />


**Step:3 Create main.tf file (To create machines kubernetes master, slave1 and slave2)******

Create the access keys -> open AWS console-> IAM
(Copy the access key and secret key)

<img width="602" height="196" alt="image" src="https://github.com/user-attachments/assets/5c84a227-9c30-4801-8baa-66590a0e8e2d" />

**Create main.tf file******

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

Run the following commands from Machine-1

Run command -> terraform init

<img width="601" height="153" alt="image" src="https://github.com/user-attachments/assets/400a5354-5d06-4e9b-95f5-74ca3b3bdc81" />

Run ->terraform plan

 <img width="602" height="86" alt="image" src="https://github.com/user-attachments/assets/b2b2cbc0-6959-47a1-821f-48345597e1cb" />

 <img width="602" height="165" alt="image" src="https://github.com/user-attachments/assets/a5f6ea1b-6029-4fb5-bffe-f01ab6b13a65" />

 Run ->terraform apply
 
 <img width="602" height="55" alt="image" src="https://github.com/user-attachments/assets/374a3b6d-857a-457d-a578-abace2b03780" />

 <img width="602" height="262" alt="image" src="https://github.com/user-attachments/assets/2b057d58-7fa8-4e50-8add-69f20ccfbdfc" />
 

Machines Kubernetes master(Machine-2), slave1(Machine-3) and slave3(Machine-4) created successfully

 <img width="602" height="124" alt="image" src="https://github.com/user-attachments/assets/074f1f43-e889-4866-a7fb-8d288c5545b6" />

 <img width="602" height="106" alt="image" src="https://github.com/user-attachments/assets/7d0866dd-9fa0-4591-bb57-7212748ab949" />

 <img width="602" height="116" alt="image" src="https://github.com/user-attachments/assets/51253f78-2dfe-4cb2-847a-826e9f462928" />

 <img width="602" height="101" alt="image" src="https://github.com/user-attachments/assets/7e77067f-af14-4724-a9eb-3063b6b239e5" />


**Step:4 Intsall Ansible in Machine-1******

Open the link-> Installing Ansible on specific operating systems — Ansible Documentation

Copy the commands and run one by one

<img width="602" height="206" alt="image" src="https://github.com/user-attachments/assets/819bd068-a18d-404f-832f-6d75cbfbe54e" />

****Generate keypair

Run command->ssh-keygen****

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

**
**Step:5 Create script files to install java, docker, kubernetes and jenkins ******

Create file script1.sh to install java and jenkins im Machine-1

Open the link and copy the code->Linux (jenkins.io)**



<img width="602" height="198" alt="image" src="https://github.com/user-attachments/assets/29561762-8329-427d-b9da-b5aa4a6d0a1b" />


<img width="409" height="24" alt="image" src="https://github.com/user-attachments/assets/9f234462-989e-42f2-8711-5a07a74b7e0a" />

sudo apt update

sudo apt install openjdk-11-jdk -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
  
sudo apt-get update

sudo apt-get install jenkins -y


<img width="602" height="165" alt="image" src="https://github.com/user-attachments/assets/008931ac-ee42-4beb-b083-a949905d91a1" />

**Create script2.sh to install java, docker and kubernetes in K8s-master machine**


<img width="433" height="21" alt="image" src="https://github.com/user-attachments/assets/e3d8d457-babc-40ca-93ca-e48c65cc7276" />

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


<img width="602" height="188" alt="image" src="https://github.com/user-attachments/assets/0b5b8a92-7d68-4ab8-bbe5-7654b8839dd0" />

**Create script3.sh to install docker and kubernetes in K8s-slave1 and K8s-slave2**


<img width="401" height="31" alt="image" src="https://github.com/user-attachments/assets/e11adeeb-51de-48f2-83fb-42edab1cb76c" />

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


<img width="602" height="143" alt="image" src="https://github.com/user-attachments/assets/052e7506-9fee-4f72-8646-8a75c76ac6be" />

**Step:6 Create playbook playbook1.yaml******


<img width="442" height="25" alt="image" src="https://github.com/user-attachments/assets/c4801830-1fe7-436b-bbd0-4590927d2053" />

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



<img width="601" height="277" alt="image" src="https://github.com/user-attachments/assets/3e701c29-0dc4-4343-9050-14b036f3028b" />

**Run command-> ansible-playbook playbook1.yaml –syntax-check**

**Run command-> ansible-playbook playbook1.yaml –check**
























 
 
 












