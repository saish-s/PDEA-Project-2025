pipeline {
    agent any
    environment {
        DOCKER_HUB_USER = 'saishshindepersistent'
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/saish-s/PDEA-Project-2025.git'
            }
        }
        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }    
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    def appImage = docker.build("${DOCKER_HUB_USER}/usermanagement:latest")
                    docker.withRegistry('', 'docker-hub-credentials') {
                        appImage.push()
                    }
                }
            }
        }
        stage('Deploy with Compose') {
            steps {
                sh 'Docker compose up -d'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'rm -rf *'
            }
        }
    }
}
