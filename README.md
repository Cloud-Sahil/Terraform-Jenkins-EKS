# Terraform-Jenkins(EKS)
## 1. Create EC2 Instances
- **Instance :** Jenkins (instance type : c7i-flex.large or more) 

## 2. Setup Jenkins EC2 Server

### Go To Root 
```sh
sudo -i
```

### Update Instance
```sh
apt update
```
### Install Java 17
```bash
apt install openjdk-17-jdk -y
```
### Install Terraform 
```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```
### Install Maven
```bash
apt install maven -y
```
### Install Jenkins

Follow official Jenkins documentation
👉 [Jenkins Installation](https://www.jenkins.io/doc/book/installing/linux/)

### Install AWS Cli
```sh
snap install aws-cli --classic
```
### Jenkins 
```sh
su - jenkins
```
### Aws Configure

## 3. Access Jenkins
- Jenkins: `http://<jenkins-public-ip>:8080`
  
## 4. Create Jenkins Pipeline
1. Go to Dashboard → New Item → Pipeline
2. Paste the following pipeline code:
```sh
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/Cloud-Sahil/Terraform-Jenkins-EKS.git'
                sh 'terraform init'
            }
        }
        stage('Test') {
            steps {
                sh 'terraform apply --auto-approve'
            }
        }
        stage('Deploy') {
            steps {
                echo "successfully deployed eks through jenkins"
            }
        }
    }
}

```

## 5. Run the Pipeline
- Build the job in Jenkins.
