# Pipeline to deploy flask app

## Various steps we shall use to deploy this pipeline -
 1. Provision a kubernetes cluster by using **Terrraform**
 2. Build and pull an **Docker** image from **DockerHub** 
 3. Create a Pipeline Using **Jenkins** to deploy an app using **Ansible**
 4. Monitor the deployment using **ELK Stack** deployed on **Docker**
