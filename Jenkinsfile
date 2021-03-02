//pipeline to run docker, Ansible and Kubernetes
pipeline {
			agent any
			environment {        
				DOCKER_HUB_REPO = "sarankaja/kubesba"
				REGISTRY_CREDENTIAL = "dockerhub"
				CONTAINER_NAME = "flaskapp"	
			}
			stages {
				stage('Clean workspace'){
					steps{
						script{
							sh 'rm -rf $PWD/case2'						
						}
					}
				}
				stage('Cloning Git'){
					steps {
						script{
							sh 'git clone https://github.com/Kajasaran/case2.git' 
						}		
					}
				}
				stage('Build docker image ') {
					steps {
						script {
							sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
							sh 'docker image tag $DOCKER_HUB_REPO:latest $DOCKER_HUB_REPO:$BUILD_NUMBER'
							echo "image buit successfuly"
						}
					}	
				}
			       stage('Push Docker Image') {
               				 steps {
                   				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){

                    						sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    	
                    						sh 'docker push sarankaja/kubesba'
                 }
             }
         }
              
   				stage('Deploy to playbook'){
					steps{
  						ansiblePlaybook(playbook: 'playbook.yaml')
  }
}

        						
			stage('kube running succesfully'){
				steps{
					script {
						sh 'kubectl get services'
					}
				}				
			}
		}
	}
