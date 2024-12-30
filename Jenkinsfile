pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "22it227/my-app:v1.0.0" // Replace with your desired name and version
    }
    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/SanjaiKumar2311/projects.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    docker login -u $DOCKER_USER -p $DOCKER_PASS
                    docker push ${DOCKER_IMAGE}
                    """
                }
            }
        }
        stage('Deploy on EC2') {
            steps {
                sshagent(['aws_credentials']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@13.233.159.187 "
                        docker login -u 22it227 -p SANjai@123 &&
                        docker pull ${DOCKER_IMAGE} &&
                        docker run -d -p 8080:80 ${DOCKER_IMAGE}"
                    """
                }
            }
        }
    }
}
