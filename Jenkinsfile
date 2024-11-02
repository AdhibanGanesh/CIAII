pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "adhibanganesh/html-app:latest"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/AdhibanGanesh/CIAII.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
        stage('Ansible Configuration') {
            steps {
                ansiblePlaybook become: true, playbook: 'ansible/deploy.yml', inventory: 'ansible/inventory.ini'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f k8s/deployment.yml"
                }
            }
        }
    }
}
