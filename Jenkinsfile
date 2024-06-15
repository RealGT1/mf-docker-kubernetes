pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'localhost:5000'
        CLIENT_IMAGE = 'mf3-client'
        SERVER_IMAGE = 'mf3-server'
        K8S_CLIENT_DEPLOYMENT = './client/client-deployment.yaml'
        K8S_SERVER_DEPLOYMENT = './server/server-deployment.yaml'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your GitHub repository
                git url: 'https://github.com/RealGT1/mf-docker-kubernetes.git', branch: 'main' // Update URL
            }
        }

        stage('Build Client Docker Image') {
            steps {
                script {
                    // Build Client Docker image
                    sh 'docker build -t ${CLIENT_IMAGE}:latest ./client'
                }
            }
        }

        stage('Build Server Docker Image') {
            steps {
                script {
                    // Build Server Docker image
                    sh 'docker build -t ${SERVER_IMAGE}:latest ./server'
                }
            }
        }

        stage('Run Client Tests') {
            steps {
                script {
                    // Run your Client tests
                    sh 'docker run --rm ${CLIENT_IMAGE}:latest npm test' // Update with actual test command
                }
            }
        }

        stage('Run Server Tests') {
            steps {
                script {
                    // Run your Server tests
                    sh 'docker run --rm ${SERVER_IMAGE}:latest pytest' // Update with actual test command
                }
            }
        }

        stage('Push Client Image to Local Registry') {
            steps {
                script {
                    // Push the Client image to your local Docker registry
                    sh 'docker tag ${CLIENT_IMAGE}:latest ${DOCKER_REGISTRY}/${CLIENT_IMAGE}:latest'
                    sh 'docker push ${DOCKER_REGISTRY}/${CLIENT_IMAGE}:latest'
                }
            }
        }

        stage('Push Server Image to Local Registry') {
            steps {
                script {
                    // Push the Server image to your local Docker registry
                    sh 'docker tag ${SERVER_IMAGE}:latest ${DOCKER_REGISTRY}/${SERVER_IMAGE}:latest'
                    sh 'docker push ${DOCKER_REGISTRY}/${SERVER_IMAGE}:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Deploy the Client and Server to Kubernetes
                    sh 'kubectl apply -f ${K8S_CLIENT_DEPLOYMENT}'
                    sh 'kubectl apply -f ${K8S_SERVER_DEPLOYMENT}'
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
            echo 'Build and Deployment succeeded!'
        }
        failure {
            echo 'Build or Deployment failed!'
        }
    }
}
