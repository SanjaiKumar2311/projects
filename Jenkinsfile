/*
pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username and tag
        DOCKER_IMAGE = "22it227/ec2:tagname"
        DOCKERHUB_CREDENTIALS = 'dockerhub_credentials'  // Use Jenkins credentials manager for Docker Hub credentials
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository
                git branch: 'master', url: 'https://github.com/SanjaiKumar2311/projects.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the current commit hash or build number
                    echo "Building Docker image: $DOCKER_IMAGE"
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub securely using Jenkins credentials
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        echo "Logging into Docker Hub"
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    }
                    // Push the Docker image to Docker Hub
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Ensure kubectl is configured to use Docker Desktop Kubernetes context
                    echo "Setting kubectl context"
                    sh 'kubectl config use-context docker-desktop'

                    // Apply Kubernetes manifests for deployment and service
                    echo "Deploying to Kubernetes"
                    sh 'kubectl apply -f kubernetes/deployment.yaml'
                    sh 'kubectl apply -f kubernetes/service.yaml'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete.'
        }
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}
*/
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "22it227/ec2:tagname" // Replace with your Docker Hub username and image tag
        DOCKERHUB_CREDENTIALS = 'dockerhub_credentials' // Replace with your Jenkins credentials ID for Docker Hub
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from repository..."
                git branch: 'master', url: 'https://github.com/SanjaiKumar2311/projects.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: $DOCKER_IMAGE"
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        echo "Logging into Docker Hub..."
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                    echo "Pushing Docker image: $DOCKER_IMAGE"
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo "Setting kubectl context to Docker Desktop..."
                    sh 'export PATH=$PATH:/usr/local/bin && kubectl config use-context docker-desktop'

                    echo "Applying Kubernetes manifests..."
                    sh '''
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                        kubectl apply -f pod.yaml
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete.'
        }
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}

