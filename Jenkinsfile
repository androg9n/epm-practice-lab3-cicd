pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                nodejs('nodejs22') {
                    sh 'npm config ls'
                }
            }
        }
    }
}
