pipeline {
    agent any
    
    tools {
        maven "3.6.3"
    }

    stages {
        stage('Build Web-App') {
            steps {
                sh "mvn clean install"
            }
        }

        stage('Build Docker image for Web-App') {
            steps {
                sh "docker build -t apoorva3091/quizapp ."
            }
        }

        stage('Upload image to Docker Registry') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub', variable: 'DockerToken')]) {
                    echo "Login to Docker Hub"
                    sh "docker login --username=apoorva3091 --password=${DockerToken}"
                    echo "Upload quizapp image to Docker Hub"
                    sh "docker push apoorva3091/quizapp:latest"
                }
            }
        }

        stage('Invoking CD Pipeline') {
            steps {
                input message: 'Do you want to approve the deployment?', ok: 'Yes'
                build job: 'quizapp_CD'
            }
        }
    }
    
    post {
        failure {
            emailext body: '${BUILD_LOG}', subject: 'Build Failed', to: 'apoorvamishra3091@gmail.com'
        }
    }
}
