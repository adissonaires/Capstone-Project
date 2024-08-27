<p align="center">
  <a href="https://airbyte.com"><img src="https://assets.website-files.com/605e01bc25f7e19a82e74788/624d9c4a375a55100be6b257_Airbyte_logo_color_dark.svg" alt="Airbyte"></a>
</p>

Airbyte provides the largest catalog of 300+ connectors for APIs, databases, data warehouses, and data lakes.

![Airbyte OSS Connections UI](https://github.com/airbytehq/airbyte/assets/10663571/870d0479-2765-4ecb-abd5-a5bb877dae37)

#

## AWS Deployment:

1. Install Docker Engine and Kubernetes plugin on your workstation. 
2. Clone the project repository from GitHub:

```jsx
git clone https://github.com/amigo-nishant/Capstone-Project-Clone.git
```
3. Navigate into the Airbyte directory:
   
```jsx
cd Capstone-Project-Clone
```

4. Start Service using helm

5. Access in your browser by visiting - http://public-IP:8000

6. You will be asked for a username and password. that's username --> airbyte and password --> password.

7. Enter basic details and login

## Deploy Airbyte on Kubernetes using Helm

1. Install Helm, kubernetes, Docker and setup cluster
2. Add Helm Repository
3. To deploy on Kubernetes, first add the Helm repository:

```jsx
helm repo add airbyte https://airbytehq.github.io/helm-charts
```
3. Then update the repository index:

```jsx
helm repo update
```
4. You can search for available Airbyte charts using:

```jsx
helm search repo airbyte
```
5. It'll produce output similar to below:
```jsx
NAME                            	CHART VERSION	APP VERSION	DESCRIPTION
airbyte/airbyte                 	0.49.9       	0.50.33    	Helm chart to deploy airbyte
airbyte/airbyte-api-server      	0.49.9       	0.50.33    	Helm chart to deploy airbyte-api-server
airbyte/airbyte-bootloader      	0.49.9       	0.50.33    	Helm chart to deploy airbyte-bootloader
airbyte/connector-builder-server	0.49.9       	0.50.33    	Helm chart to deploy airbyte-connector-builder-...
airbyte/cron                    	0.49.9       	0.50.33    	Helm chart to deploy airbyte-cron
airbyte/metrics                 	0.49.9       	0.50.33    	Helm chart to deploy airbyte-metrics
airbyte/pod-sweeper             	0.49.9       	0.50.33    	Helm chart to deploy airbyte-pod-sweeper
airbyte/server                  	0.49.9       	0.50.33    	Helm chart to deploy airbyte-server
airbyte/temporal                	0.49.9       	0.50.33    	Helm chart to deploy airbyte-temporal
airbyte/webapp                  	0.49.9       	0.50.33    	Helm chart to deploy airbyte-webapp
airbyte/worker                  	0.49.9       	0.50.33    	Helm chart to deploy airbyte-worker
```

6. Deploy Airbyte, default Deployment and if you don't need to customize your deployment, you can deploy Airbyte with default values. Run the following command:

```jsx
helm install release_name airbyte/airbyte --dry-run --debug
```

7. Replace **release_name** with your desired release name. The release name should only contain lowercase letters and dashes, and it must start with a letter.


# DevSecOps Project
## Introduction
Welcome to our open source DevSecOps project! This repository serves as an educational resource for developers who want to learn how to deploy a micro-service application to **PRODUCTION** using DevSecOps principles. 

### Tools Covered:
-  Linux
-  Git and GitHub
-  Docker
-  Kubernetes
-  Docker-compose
-  Jenkins CI/CD
-  SonarQube Scan
-  SonarQube Quality Gates
-  Trivy
-  Prometheus & Grafana

## Pre-requisites to implement this project:

-  AWS EC2 instance (Ubuntu) with instance type t2.large and root volume 29GB.

-  Installation of JAVA
   ```bash
   sudo apt update
   sudo apt install fontconfig openjdk-17-jre
   java -version
   ```

-  Installation of Jenkins
   ```bash
   sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
   https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
   /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt-get update
   sudo apt-get install jenkins
   ```
-  Setup Jenkins
   ```bash
   Public-IP:8080 (Jenkins running)
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
-  Docker and docker-compose installled
   ```bash
    sudo apt-get update
    sudo apt-get install docker.io -y
    sudo apt-get install docker-compose -y
    sudo usermod -aG docker $USER
    sudo reboot
   ```
- Install trivy --> https://aquasecurity.github.io/trivy/v0.18.3/installation/

- SonarQube Server installed
   ```bash
    docker run -itd --name sonarqube-server -p 9000:9000 sonarqube:lts-community
   ```
#
## Steps for Jenkins CI/CD:

1)  Access Jenkins UI and setup Jenkins

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/1eec417e-95ab-4497-ad31-443ecd6b999e)

#

2)  Plugins Installation:

    - Go to <b><i><u>Manage Jenkins</u></i></b>, click on <b><i><u>Plugins</u></i></b> and install all the plugins listed below, we will require for other tools integration:

        - SonarQube Scanner (Version2.16.1)
        - Sonar Quality Gates (Version1.3.1)
        - Docker (Version1.5)
        - Kubernetes
#

3) Go to SonarQube Server and create token

    - Click on <b><i><u> Administration </u></i></b> tab, then <b><i><u> Security </u></i></b>, then <b><i><u> Users </u></i></b> and create Token.
    -  Create a webhook to notify Jenkins that Quality gates scanning is done. (We will need this step later)

        - Go to SonarQube Server, then <b><i><u> Administration </u></i></b>, then <b><i><u> Configuration </u></i></b> and click on <b><i><u> Webhook </u></i></b>, add webhook in below <b>Format</b>:
        > http://<jenkins_url>:8080/sonarqube-webhook/
        
        Example: 

        ```bash
            http://34.207.58.19:8080/sonarqube-webhook/
        ```

        ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/b9ef2301-b8ff-46f4-a457-6345d5e2dab6)


        ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/08a33164-f6a6-4c5d-8a34-7091cf8a5745)

#

4) Go to Jenkins UI <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> Credentials </u></i></b> and add SonarQube Credentials.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/f6db72ec-7d8c-4f4c-ae7a-55d99dd20ce9)

#

5) Now, It's time to integrate SonarQube Server with Jenkins, go to <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> System </u></i></b> and look for <b><i><u> SonarQube Servers </u></i></b> and add SonarQube.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/54849cb2-fe56-4acd-972d-3057a0eb3deb)

#

6) Go to <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> tools </u></i></b>, look for <b><i><u> SonarQube Scanner installations </u></i></b> and add SonarQube Scanner.

> Note: Add name as ```Sonar```
![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/1fe926f6-a844-42d4-bce4-62193dde6640)

#


### Configure the Jenkins Job PROD 
- create a jenkins job "PROD deployment"
- select pipeline
- Pipeline -> SCM --> credentials --> Username and PAT --> Secret name "jenkins-secret"
- ALso before triggering the build check make sure to give jenkins and docker permission 
 ```bash
sudo usermod -aG docker jenkins
sudo systemctl restart docker
sudo systemctl restart jenkins
docker ps -a
docker start <sonarqube-container-ID>
Trigger the pipeline
```
### To start jenkins as a user for PROD environment
- ssh to jenkins instance
- sudo su jenkins
- cd /var/lib/jenkins/workspace/<your-job>
- mkdir -p ~/.docker/cli-plugins/
- curl -SL https://github.com/docker/compose-cli/releases/download/v2.0.0-rc.1/docker-compose-linux-amd64 -o ~/.docker/cli-plugins/docker-compose
- chmod +x ~/.docker/cli-plugins/docker-compose
- Trigger the pipeline
- Thr WebApp service is exposed on port 8000
- Username - "airbyte" & password "password"
- Create plugins and test connectors.
