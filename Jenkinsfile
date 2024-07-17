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
        stage('Build Image') {
            steps {
                script {
                    docker.build("nodemain:v1.0", ".")
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    sh "docker stop \$(docker ps -q --filter 'ancestor=nodemain:v1.0') 2>null || echo 'Does not exist containers from image nodemain:v1.0'"
                    sh "docker rm \$(docker ps -aq --filter 'ancestor=nodemain:v1.0') 2>null || echo 'Does not exist containers from image nodemain:v1.0'"
                    docker.image('nodemain:v1.0').run('--expose 3000 -p 3000:3000')
                }
            }
        }

    }
}
