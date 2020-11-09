# Apache-Airflow

## Airflow

## Installation steps

### 1) Copying dag files from git repo
When you build Docker Image using the Dockerfile, please add a git clone command so that the dag files are copied into the container. Currently, airflow configuration file is set as */usr/local/airflow/airflow.cfg* and the dag folder is mentioned as **/usr/local/airflow/dags**
For Eg:
```shell
WORKDIR /usr/local/airflow/dags
RUN git clone <git repo>
```
### 2) Creating configmap for Airflow configuration file
Execute below command to create a configmap for airflow configuration file.
```shell
# kubectl create configmap airflow.cfg --from-file=airflow.cfg
```
where **airflow.cfg** file  is the configuration file.

### 3) Change image name from deployment files
Update docker image name in below given deployment files to the newly created docker image.

**scheduler-deployment.yaml**
**webserver-deployment.yaml**
**flower-deployment.yaml**
**worker-deployment.yaml**

```shell
        image: chiju/airflow
 ```
### 4) For creating deployments
Execute **kubectl create -f .** from the folder containing deployment files
```shell
# pwd
/root/apache-airflow
# ls
 airflow.cfg
 flower-deployment.yaml
 flower-service.yaml
 postgres-deployment.yaml
 postgres-service.yaml
 redis-deployment.yaml
 redis-service.yaml
 scheduler-deployment.yaml
 webserver-deployment.yaml
 webserver-service.yaml
 worker-deployment.yaml
 worker-service.yaml
# kubectl create -f .
```



## Kubernetes deployment details

### Deployment files

-> 1 webserver deployment which will create webserver pod
-> 1 scheduler deployment which will create sheduler pod
-> 1 flower deployment which will create  flower pod
-> 1 postgres deployment which will create postgres pod
-> 1 redis deployment which will create redis pod
-> 1 worker deployment which will create worker pod

### For increasing the number of workers

Currently, the number of worlers is set to be **1** in workers deployment file and it can be increased by changing the **replica** parameter value in **worker-deployment.yaml** file
```shell
  replicas: 1
```
## For accessing Airflow UI
```html
http://serverIP:31050
```
