//pipeline to run docker, Ansible and Kubernetes
pipeline {
			agent any
			
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
				
	 
   				stage('Deploy to playbook'){
					steps{
  						ansiblePlaybook installation: 'Ansible', playbook: 'playbook.yaml'
  }
}

        						
			stage('kube running succesfully'){
				steps{
					script {
						sh '/usr/local/bin/kubectl get services'
					}
				}				
			}
		}
	}
