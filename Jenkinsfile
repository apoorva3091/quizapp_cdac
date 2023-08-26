pipeline {
agent any
	
tools {
maven "3.6.3"
}

stages {  
    stage('Build'){
        steps{
                sh "mvn clean install"
		}
        }
        
    stage ('Build Docker image') {
        steps{
		sh "docker build -t apoorva3091/quizapp ."
		  }       
        }

    stage('Upload image to Docker Registry'){
        steps{
            	echo "Docker Login"
		withCredentials([string(credentialsId: 'dockerhub', variable: 'DockerToken')]) {
			sh "docker login --username=apoorva3091 --password=${DockerToken}"
			echo "Docker push"
			sh "docker push apoorva3091/quizapp:latest"
		}
            }
        }

    stage('Invoking CD Part'){
        steps{
            build job: 'quizapp-CD'
            }
        }
    }
    
post {
    failure {
        emailext body: '${BUILD_LOG}', subject: 'Build Failed', to: 'apoorvamishra3091@gmail.com'
        }
    }
}
