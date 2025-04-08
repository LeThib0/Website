pipeline {
	agent any
 
				stages {
			//---------------------------------------
			    	stage('git version') {
			        		steps {
			        		    sh 'git version'
			        		}
			    	}
			//---------------------------------------
			    	stage('Docker Build & Push') {
        steps {
            withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD_', variable: 'DOCKER_HUB_PASSWORD')]) {
                
                sh 'docker login -u thib432 -p $DOCKER_HUB_PASSWORD'
                sh 'docker build -t thib432/devops-mywebsite:v1 .'
                sh 'docker push thib432/devops-mywebsite:v1'
            }
        }
    }
			//---------------------------------------
			   	stage('kubernetes version') {
			        		steps {   
			        		    withKubeConfig([credentialsId: 'KuberneteThib']) {
			        		    	sh 'kubectl version --client'  	
			        		    }
			        		}
			    	}
			//---------------------------------------
 
 
	}
}