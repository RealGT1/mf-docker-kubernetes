pipeline {
    agent any

    environment {
        DOCKER_HOST = 'tcp://localhost:2375'
        CLIENT_IMAGE = 'mf3-client'
        SERVER_IMAGE = 'mf3-server'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your GitHub repository
                git url: 'https://github.com/RealGT1/mf-docker-kubernetes.git', branch: 'main'
            }
        }

        stage('Build Client Docker Image') {
            steps {
                script {
                    // Build Client Docker image
                    docker.build("${CLIENT_IMAGE}:latest", "./client")
                }
            }
        }

        stage('Build Server Docker Image') {
            steps {
                script {
                    // Build Server Docker image
                    docker.build("${SERVER_IMAGE}:latest", "./server")
                }
            }
        }

        stage('Run Client Tests') {
            steps {
                script {
                    // Run your Client tests
                    docker.image("${CLIENT_IMAGE}:latest").run("--rm", "npm", "test")
                }
            }
        }

        stage('Run Server Tests') {
            steps {
                script {
                    // Run your Server tests
                    docker.image("${SERVER_IMAGE}:latest").run("--rm", "pytest")
                }
            }
        }

        stage('Push Client Image to Local Registry') {
            steps {
                script {
                    // Push the Client image to your local Docker registry
                    docker.withRegistry("http://${DOCKER_HOST}") {
                        docker.image("${CLIENT_IMAGE}:latest").push()
                    }
                }
            }
        }

        stage('Push Server Image to Local Registry') {
            steps {
                script {
                    // Push the Server image to your local Docker registry
                    docker.withRegistry("http://${DOCKER_HOST}") {
                        docker.image("${SERVER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker resources
            sh 'docker system prune -f'
        }
        success {
            echo 'Build and image push succeeded!'
        }
        failure {
            echo 'Build or image push failed!'
        }
    }
}
