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
			stage('Kubernetes Deployment') {
    steps {
        withKubeConfig([credentialsId: 'KuberneteThib']) {
            script {
                // Utilisation de guillemets doubles pour éviter les erreurs d'interprétation
                sh "sed -i \"s#replace#thib432/devops-mywebsite:v1#g\" k8s_deployment_service.yaml"
                sh 'kubectl apply -f k8s_deployment_service.yaml'
            }
        }
    }
}
    stage('Force Rollout Restart') {
    steps {
        withKubeConfig([credentialsId: 'KuberneteThib']) {
            script {
                // This forces Kubernetes to restart the deployment and pull the latest image
                sh 'kubectl rollout restart deployment devthib'
            }
        }
    }
}

}
}