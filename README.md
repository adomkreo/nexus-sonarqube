# nexus
1- Install helm: sudo snap install helm -- classic
2- Create a namespace (sonarqube)
3- helm repo add sonatype https://sonatype.github.io/helm3-charts/
4- helm repo update
5- helm install nexus sonatype/nexus-repository-manager -n sonarqube
6- create ingress (after creating A) record


apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nexus-ingress 
spec:
  ingressClassName: nginx
  rules:
  - host: nexus.smartuniversaldevops.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nexus-rm-nexus-repository-manager
            port:
              number: 8081
  #############################################################
  # Installing nexus on an instance

Nexus Installation And Setup In AWS EC2 Redhat Instance.

Pre-requisite

AWS Acccount.
Create Redhat EC2 t2.medium Instance with 4GB RAM.
Create Security Group and open Required ports.
8081 ..etc
Attach Security Group to EC2 Instance.
Install java openJDK 1.8+ for Nexus version 3.15
Create nexus user to manage the Nexus server

#As a good security practice, Nexus is not advised to run nexus service as a root user, 
# so create a new user called nexus and grant sudo access to manage nexus services as follows. 
sudo hostnamectl set-hostname nexus
sudo useradd nexus
# Grand sudo access to nexus user
sudo echo "nexus ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/nexus
sudo su - nexus
Install Java as a pre-requisit for nexus and other softwares

cd /opt
sudo yum install wget git nano unzip -y
sudo yum install java-11-openjdk-devel java-1.8.0-openjdk-devel -y
Download nexus software and extract it (unzip).

sudo wget http://download.sonatype.com/nexus/3/nexus-3.15.2-01-unix.tar.gz 
sudo tar -zxvf nexus-3.15.2-01-unix.tar.gz
sudo mv /opt/nexus-3.15.2-01 /opt/nexus
sudo rm -rf nexus-3.15.2-01-unix.tar.gz
Grant permissions for nexus user to start and manage nexus service

# Change the owner and group permissions to /opt/nexus and /opt/sonatype-work directories.
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
sudo chmod -R 775 /opt/nexus
sudo chmod -R 775 /opt/sonatype-work
Open /opt/nexus/bin/nexus.rc file and uncomment run_as_user parameter and set as nexus user.

# change from #run_as_user="" to [ run_as_user="nexus" ]

echo  'run_as_user="nexus" ' > /opt/nexus/bin/nexus.rc
CONFIGURE NEXUS TO RUN AS A SERVICE

sudo ln -s /opt/nexus/bin/nexus /etc/init.d/nexus

#9 Enable and start the nexus services
sudo systemctl enable nexus
sudo systemctl start nexus
sudo systemctl status nexus
echo "end of nexus installation"
  
  ###############################################################
  # sonarqube 9.9 LTS CHART
1- helm repo add sonatype https://sonatype.github.io/helm3-charts/ 
2- helm repo update
3- helm upgrade --install -n sonarqube --version ~8 sonarqube sonarqube/sonarqube
4- create ingress (after creating A record)

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sonarqube-ingress 
spec:
  ingressClassName: nginx
  rules:
  - host: sonarqube.smartuniversaldevops.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: sonarqube-sonarqube
            port:
              number: 9000
  ###################################################################################
# Instanling sonarqube on an instance
## SonarQube Installation And Setup In AWS EC2 Redhat Instance.
##### Prerequisite
+ AWS Acccount.
+ Create Redhat EC2 T2.medium Instance with 4GB RAM.
+ Create Security Group and open Required ports.
   + 9000 ..etc
+ Attach Security Group to EC2 Instance.
+ Install java openJDK 1.8+ for SonarQube version 7.8

## 1. Create sonar user to manage the SonarQube server
```sh
#As a good security practice, SonarQuber Server is not advised to run sonar service as a root user, 
# create a new user called sonar and grant sudo access to manage sonar services as follows

sudo useradd sonar
# Grand sudo access to sonar user
sudo echo "sonar ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/sonar
# set hostname for the sonarqube server
sudo hostnamectl set-hostname sonar 
sudo su - sonar
```
## 1b. Assign password to sonar user
```sh
sudo passwd sonar
```
## 2. Enable PasswordAuthentication in the server
```sh
sudo sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config
sudo service sshd restart
```
### 3. Install Java JDK 1.8+ required for sonarqube to start

``` sh
cd /opt
sudo yum -y install unzip wget git
sudo yum install  java-11-openjdk-devel -y 
```
### 4. Download and extract the SonarqQube Server software.
```sh
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.8.zip
sudo unzip sonarqube-7.8.zip
sudo rm -rf sonarqube-7.8.zip
sudo mv sonarqube-7.8 sonarqube
```

## 5. Grant file permissions for sonar user to start and manage sonarQube
```sh
sudo chown -R sonar:sonar /opt/sonarqube/
sudo chmod -R 775 /opt/sonarqube/
```
### 6. start sonarQube server
```sh
sh /opt/sonarqube/bin/linux-x86-64/sonar.sh start 
sh /opt/sonarqube/bin/linux-x86-64/sonar.sh status
```

### 7. Ensure that SonarQube is running and Access sonarQube on the browser
# sonarqube default port is = 9000
# get the sonarqube public ip address 
# publicIP:9000
```sh
curl -v localhost:9000
54.236.232.85:9000
default USERNAME: admin
default password: admin
```
