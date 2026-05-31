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
        stage('Check Files') {
            steps {
                sh 'ls -ltr'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t mysite .'
            }
        }
        stage('Docker Run') {
            steps {
                sh 'docker stop mysite || true'
                sh 'docker rm mysite || true'
                sh 'docker run -d -p 80:80 --name mysite mysite'
            }
        }
    }
}
