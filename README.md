# Pipeline to deploy flask app

## Various steps we shall use to deploy this pipeline -
 1. Provision a kubernetes cluster by using **Terrraform**
 2. Build and pull an **Docker** image from **DockerHub** 
 3. Create a Pipeline Using **Jenkins** to deploy an app using **Ansible**
 4. Monitor the deployment using **ELK Stack** deployed on **Docker**


# Project workflow
![Image](https://github.com/kajasaran/case2/blob/master/Screen_shots/Screen%20Shot%202021-03-03%20at%2011.41.14%20AM.png)

The above image depicts the flow of our pipeline
we will use terraform to provision a kind cluster. Once the cluster is provisioned we link our jenkins to git to pull the code from there and deploy the app using ansible. Ansible will deploy the app based on the instructions in playbook on a kubernetes cluster. We then deploy ELK Stack on a seperate container and configure it to monitor system logs and docker logs.

# Let's See all the steps in detail

# Step-1 Provisioning a Kubernetes Cluster

1. Create a folder where you will place all the files related to this deployment.
2. Create [main.tf](https://github.com/kajasaran/case2/blob/master/main.tf) file, which contains all the instructions to provision the cluster.
3. Use `terraform init` to intialize terraform in that folder.
