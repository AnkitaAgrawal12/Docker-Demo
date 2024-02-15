pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'calculator'
        GITHUB_REPO_URL = 'https://github.com/AnkitaAgrawal12/Docker-Demo.git'
    }
    stage('Cleanup') {
            steps {
                // Remove specific Docker containers
                sh 'docker rm -f calculator'
                
                // Or, you can remove containers by their IDs
                // sh 'docker rm -f $(docker ps -aqf "name=container1")'
                // sh 'docker rm -f $(docker ps -aqf "name=container2")'
            }
        }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the GitHub repository
                    git branch: 'main', url: "${GITHUB_REPO_URL}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build("${DOCKER_IMAGE_NAME}", '.')
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script{
                    docker.withRegistry('', 'DockerHubCred') {
                    sh 'docker tag calculator ankitaagrawal12/calculator:latest'
                    sh 'docker push ankitaagrawal12/calculator'
                    }
                 }
            }
        }

   stage('Run Ansible Playbook') {
            steps {
                script {
                    ansiblePlaybook(
                        playbook: 'deploy.yml',
                        inventory: 'inventory'
                     )
                }
            }
        }

    }
}
