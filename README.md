# Pipeline to deploy flask app

## Various steps we shall use to deploy this pipeline -
 1. Provision a kubernetes cluster by using **Terrraform**
 2. Build and pull an **Docker** image from **DockerHub** 
 3. Create a Pipeline Using **Jenkins** to deploy an app using **Ansible**
 4. Monitor the deployment using **ELK Stack** deployed on **Docker**


# Prerequisites
Make sure you have all these installed
1.Java 
2.Python
3.Git
4.Jenkins
5.Docker
6.Kubernetes
7.Terraform
8.Ansible
9.Elk Stack



# Project workflow
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%2011.41.14%20AM.png)

The above image depicts the flow of our pipeline
we will use terraform to provision a kind cluster. Once the cluster is provisioned we link our jenkins to git to pull the code from there and deploy the app using ansible. Ansible will deploy the app based on the instructions in playbook on a kubernetes cluster. We then deploy ELK Stack on a seperate container and configure it to monitor system logs and docker logs.

# Let's See all the steps in detail

# Step-1 Provisioning a Kubernetes Cluster

1. Create a folder where you will place all the files related to this deployment.
2. Create [main.tf](https://github.com/kajasaran/case2/blob/master/main.tf) file, which contains all the instructions to provision the cluster.
3. Use `terraform init` to intialize terraform in that folder.
4. Initialize and push the changes to git.
5. Use `terraform apply` to commit the changes and provision the cluster.
6. you can check if the cluster is provisioned by running `kubectl get all`

**output of successful provison**
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%203.16.21%20PM.png)

use [this](https://learn.hashicorp.com/tutorials/terraform/kubernetes-provider?in=terraform/kubernetes![image](https://user-images.githubusercontent.com/48532068/109850015-0a632700-7c20-11eb-921a-2c4cb16fc2d7.png)
) documnetation if you dont have kubernetes already installed.

# Step-2 Create Ansible Playbook
1. Create the file [playbook.yaml](https://github.com/kajasaran/case2/blob/master/playbook.yaml) which has all the instructions related to the deployment.
2. Create the file [kubernetes.yaml](https://github.com/kajasaran/case2/blob/master/kubernetes.yaml) this has the instructions to pull the image from DockerHib and deploy the image on pods.
3. we are calling this kubernetes.yaml file in ansible playbook to deploy it to the pods.

# Step-3 Configuring Jenkins and deploying App

1. Make sure you have Jenkins installed. Then run Jenkins by using `brew services start jenkins`
![image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-01%20at%2010.40.18%20AM.png)

2. Navigate to the port where jenkins is hosted. In my case it is [http://localhost:8080](http://localhost:8080)
3. sign in into Jenkins and create a new pipeline
4. configure it by giving you git link

**Jenkins login page**
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%201.20.22%20PM.png)

**Select pipeline and give it a name**

![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%201.20.48%20PM.png)


**click on configure**

![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%201.27.58%20PM.png)


**scroll down and choose *GIT* in SCM and give your repository name in Repository URL**

![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%201.23.24%20PM.png)


**In Script Path type `Jenkinsfile`**

![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%201.23.34%20PM.png)

**clck on apply and save**
**Then click on `Build Now`**

![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%201.27.58%20PM.png)


**You should now see a successfully built pipeline**

![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-02%20at%202.29.13%20PM.png)


**check if the services and pods are running successfully by using the commands**

`kubectl get pods`
`kubectl get services`
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-01%20at%2011.43.26%20AM.png)

**Check if the application is deployed by navigating to [localhost:30201](http://localhost:30201)
You should see the below app running**
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%2011.06.28%20AM.png)

# The app is successfully deployed and running!!

# Step-4 Monitoring

1. We are deploying ELK Stack to monitor our services.
2. Make sure you have installed [ELK Stack](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-elastic-stack.html)
3. Navigate to the folder where Elk Stack is cloned
4. run `docker-compose up`
5. you will see logs in the terminal
6. Navigate to the kibana dashboard[localhost:5601](http://localhost:5601)

## You should see the below page
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%2011.06.44%20AM.png)

**Install Metricbeat to pull the docker and system logs.If you dont have it installed plese follow [these](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-installation-configuration.html) instructions**

7. Start metricbeat by using 
   `brew services start elastic/tap/metricbeat-full`

8. Enable modules to pull logs by using the below commands
  `metricbeat modules enable docker`
  `metricbeat modules enable Kubernetes`
  `metricbeat modules enable systemlogs`
  we are enabling these to pull docker, kubernetes and systemlogs.
  
9. run `metricbeat setup -e` to configure the changes 


## ELK Stack Dashboard
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-01%20at%2012.16.40%20PM.png)
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-01%20at%2012.17.43%20PM.png)


## To monitor the performance of the machine 
1. use `top`

![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-02%20at%208.57.04%20PM.png)


## To add Stress
1. Install Stress



## Problems faced 

1. Wasnt able to log Kubernetes Metrics
![Image](Screen Shot 2021-03-03 at 7.07.27 PM.png)


  








