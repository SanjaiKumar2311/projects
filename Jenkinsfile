pipeline {
    agent any
    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/SanjaiKumar2311/projects.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t 22it227/ec2:tagname .' 
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'docker push 22it227/ec2:tagname'
            }
        }
        stage('Deploy on EC2') {
            steps {
                sshagent(['aws_credentials']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@13.126.166.3 " 
                        docker pull 22it227/ec2:tagname &&
                        docker run -d -p 8080:80 22it227/ec2:tagname"
                    '''
                }
            }
        }
    }
}
