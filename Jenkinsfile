pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username and tag
        DOCKER_IMAGE = "22it227/ec2:tagname"
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
                // Build the Docker image
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the Docker image to Docker Hub
                sh 'docker login -u 22it227 -p SANjai@123'
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Set kubectl context to Docker Desktop
                sh 'kubectl config use-context docker-desktop'
                
                // Apply Kubernetes manifests
                sh 'kubectl apply -f kubernetes/deployment.yaml'
                sh 'kubectl apply -f kubernetes/service.yaml'
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
