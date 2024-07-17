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
                    def PORT_NUMBER = "3000"
                    sh """
                    #!/bin/bash
                    CONTAINER_IDS=\$(docker ps --format '{{.ID}} {{.Ports}}' | awk '/0.0.0.0:'\$PORT_NUMBER'/ {print \$1}')
                    if [ -z "\$CONTAINER_IDS" ]; then
                        echo "No running containers found for port: \$PORT_NUMBER"
                        exit 0
                    fi
                    echo "Stopping containers..."
                    docker stop \$CONTAINER_IDS
                    echo "Removing containers..."
                    docker rm \$CONTAINER_IDS
                    echo "All containers exposing port \$PORT_NUMBER have been stopped and removed."
                    """
                    docker.image('nodemain:v1.0').run('--expose 3000 -p 3000:3000')
                }
            }
        }

    }
}
