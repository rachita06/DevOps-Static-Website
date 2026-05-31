pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rachita06/DevOps-Static-Website.git',
                    credentialsId: 'git-06'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t rachita06/mysite:latest .'
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-06',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push rachita06/mysite:latest'
                }
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@3.136.44.122 "
                            docker stop mysite || true
                            docker rm mysite || true
                            docker pull rachita06/mysite:latest
                            docker run -d -p 80:80 --name mysite rachita06/mysite:latest
                        "
                    '''
                }
            }
        }
    }
}
