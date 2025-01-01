pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username
        DOCKER_IMAGE = "22it227/ec2:tagname"
        KUBE_CONFIG = "/path/to/your/kubeconfig" // Path to kubeconfig file for Kubernetes cluster access
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository
                git branch: 'master', url: 'https://github.com/your-repo/quiz-app.git'
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
                sh 'docker login -u your-dockerhub-username -p your-dockerhub-password'
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Deploy to Kubernetes cluster
                withKubeConfig([credentialsId: 'kubeconfig-credentials', serverUrl: 'https://your-kubernetes-cluster-url']) {
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
