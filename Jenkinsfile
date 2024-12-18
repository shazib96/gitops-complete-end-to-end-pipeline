pipeline{
    agent{
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "complete-prodcution-end-to-end-pipeline"
    }

    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/shazib96/gitops-complete-end-to-end-pipeline'
            }

        }


        stage("Trigger CD Pipeline") {
            steps {
		sh """
		    cat deployment.yaml
		    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_NAME}/g' deployment.yaml
		    cat deployment.yaml
                """
            }
        }
      
        stage(Push the changed deployment file to Git){
	    steps {
	        sh """
                    git config --global user.name "shazib96"
                    git config --global user.email "shaojoiya@gmail.com"
                    git add deployment.yaml
                    git commit -m "Update Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                    sh "git push https://github.com/shazib96/gitops-complete-end-to-end-pipeline main" 
                }	
            }
	}
    }
}
