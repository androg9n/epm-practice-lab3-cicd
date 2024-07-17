pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                nodejs('nodejs22') {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                nodejs('nodejs22') {
                    sh 'npm test'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("nodedev:v1.0", ".")
                }
            }
        }
    }
}
