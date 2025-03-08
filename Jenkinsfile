pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/sanjula77/CI-CD-with-Jenkins-and-Docker.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t sanjula77/my_node_app:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'sanjula-docker', variable: 'docker_pass')]) {
                    script {
                        bat "docker login -u sanjula77 -p %docker_pass%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push sanjula77/my_node_app:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
