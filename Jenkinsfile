pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/PraveenAththanayake/docker-jenkins-ci-cd.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t praveenaththanayake/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
               withCredentials([string(credentialsId: 'praveen', variable: 'praveen')]) {
                    script {
                        bat "docker login -u praveenaththanayake -p %praveen%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push praveenaththanayake/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
