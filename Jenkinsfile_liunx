pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/VishSeran/DevOpsAssignment02.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t VishSeran/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'dockerhubpassword')]) {
                    script {
                        bat "docker login -u seranvishwa -p %dockerhubpassword%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push VishSeran/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
