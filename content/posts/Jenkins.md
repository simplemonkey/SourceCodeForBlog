---
title: "Deploy a simple hello world node.js app to AWS EKS end-end using Jenkins"
date: "2021-04-24"
description: ""

# Theme-Defined params
thumbnail: "img/cicd1.png" # Thumbnail image
lead: "Jenkins automaion "
---

﻿**Jenkins pipeline to build and deploy app on AWS EKS**


**Steps to deploy app on EKS**

1. Create Bitbucket repo with Dockerfile and Jenkinsfile

2. Configure and run Jenkins on EC2

3. Configure Bitbucket Server integration , Docker and Kubernetes plug-ins

4. Install Docker, Git, Kubectl, Eksctl on EC2 in *usr/*local/bin

    Add Jenkins to docker group

    Update $PATH=$PATH:/usr/local/bin for Jenkins user to access installed binaries

5. Create Jenkins pipeline :

     *	Connect SCM and configure build trigger to poll SCM (configure bitbucket creds and specifyJenkinsFile path in SCM)   

     * Create EKS cluster with one node

     * Build docker image

     * Push docker image to ECR

     * Deploy app and load balancer on EKS

     * Get Loadbalancer’s DNS name to access app



**1. Create hello world node.js app and commit to private bitbucket repo**



Sample code at https://saradagss@bitbucket.org/saradagss/activity1.git

(use VScode for version control )





**2. Configure and run Jenkins on EC2**

2.1 launch EC2 instance with security group for ports open on  22(ssh), 8080(for jenkins), 80(http)

2.2 Login to EC2
```ssh -i "sarada-activity.pem" <ec2-user@ec2-18-224-3-229.us-east-2.compute.amazonaws.com>```

2.3 Install Jenkins

https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/

2.4 Configure admin



**3. Configure Bitbucket Server Integration , Docker and Kubernetes plug-ins**














**4. Install Docker, Git, Kubectl, Eksctl on EC2 in *usr/*local/bin**

<https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html>



**5. Create Jenkins pipeline**

**5.1 Create new pipeline and connect SCM and provide Jenkinsfile in the scriptPath.**

Provide bitbucket credentials if its a private repo





Configure build triggers - poll scm /build when a change is pushed to bitbucket or cron job to schedule every 1 hour



**Cron job to build pipeline every hour**







**Poll SCM**

Check SCM every 2 minutes and trigger build only if any change in SCM

**5.2 Create EKS cluster with one node**
```
stage('Deploy eks cluster ') {
steps {
sh 'eksctl create cluster --name sarada-activity-eks --region us-east-2 --nodes-min 1 --node-type t3.medium'
sh 'aws eks update-kubeconfig --name sarada-activity-eks'
}
}
```

Issue faced : Jenkins not able to run kubectl/eksctl command

Added path variable in global environment variable

Manage Jenkins → Confiure System → Global Properties









Update kubeconfig for cluster for kubectl to access cluster

  ```aws eks update-kubeconfig --name```

  ```https://docs.aws.amazon.com/cli/latest/reference/eks/update-kubeconfig.html```

  ```IAM role to use  - instance role```

**5.3 Build docker image**

```
stage('Building image') {
steps{
script {
dockerImage = docker.build registry
sh 'docker images'
}
}
}
```

Issue: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock

add the jenkins user into docker group

sudo usermod -aG docker jenkins

**5.4 Push docker image to ECR**

```stage('Pushing to ECR') {
steps{
script {
sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 519852036875.dkr.ecr.us-east-2.amazonaws.com'
sh 'docker push 519852036875.dkr.ecr.us-east-2.amazonaws.com/sarada-helloword'
}
}
}
```

To Confiure sercret access key id and password for jenkins

```aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 519852036875.dkr.ecr.us-east-2.amazonaws.com```

Reference : https://dev.to/hung5s/an-easy-way-to-push-docker-image-from-jenkins-to-ecr-4lpi

**5.5 Deploy app and load balancer on EKS**

Create manifest files for deployment and loadbalancer service with target Port exposed on port 3000

and commit it to repo

```
stage('deploy app on eks with loadbalancer') {
steps{
script {
sh 'kubectl create -f dep.yaml'
sh 'kubectl create -f lb-service.yaml '
// wait till dns generates
sh 'sleep 1m'
}
}
}
```


**5.6 Get LoadBalancer DNS**

Create deployment with amazon ecr image and service of LoadBalncer type to expose the app on port 80 and container targetport 3000.

```stage('Get service DNS name') {
steps{
script {
// get svc DNS
sh 'kubectl get service/app-deployment-service'
}
}
}
```

**Pipeline view from Jenkins :**





**Check Console Output to get DNS**



**Open the URL**

