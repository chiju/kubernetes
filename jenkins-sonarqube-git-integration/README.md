# Jenkins-Sonarqube-GitHub Intergration

## Prerequisites

Jenkins, Sonarqube, Github repo

## Installing sonarqube 

### Pull docker image 
```shell
docker pull library/sonarqube:latest
```

### Run sonarqube container 
```shell
docker run -d -p 9000:9000 sonarqube
```

### Accessing sonarqube

**URL for sonarqube**
```
http://<IP Address>:9000/
```

##  Generate token from sonarqube
Administrator >> My Account >> Security >> Generate Tokens
Token will be similer to the below given key
```shell
951b72d19a4e6dda48a015281ed926871ab73d4a
```
## Create webhook in Github repository
Settings of repository (not account settings) >> Webhooks >> Add Webhook 

-> Payload URL example
```shell
http://13.233.109.59:8080/github-webhook/
```
-> Content type
```shell
application/json
```
-> Which events would you like to trigger this webhook
```shell
Just the push event
```

## Jenkins settings

### ->Adding sonarqube server
1) Install **SonarQube Scanner for Jenkins** plugin 

2) Add Sonarqube server details in jenkins as below
	Manage Jenkins >> Configure system >> SonarQube servers

Name
```shell
sonarqube
```
Server URL
```shell
http://13.233.109.59:9000
```
3) Add sonarqube token as a secret text and select this as **Server authentication token** 

### ->Global Tool Configuration

#### Adding SonarQube scanner agent
Manage Jenkins >> Global Tool Configuration >> SonarQube scanner >> Name as **sonarqube** >> Tick **Install automatically**

#### Adding Git agent
Manage Jenkins >> Global Tool Configuration >> Git >> Name as **Git** >> Tick **Install automatically**

### -> Add Job
1) Create a new job by following below steps 
New Item >> Enter an item name >> Freestyle project >> Source Code Management >> Select **Git** 

Repository URL
```shell
https://github.com/chijudevops/testing.git
```
2) Create git credetial as **username with password** credential and give that for **Credentials**

3) Tick **GitHub hook trigger for GITScm polling**

4) Add build step >> Select **Execute SonarQube Scanner**

Analysis properties
```shell
sonar.projectKey=sonar
```
