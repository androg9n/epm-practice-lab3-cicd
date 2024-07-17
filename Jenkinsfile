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
                    def imageName = "nodemain:v1.0"
                    sh """
                    #!/bin/bash
                    CONTAINER_IDS=\$(docker ps -q --filter "ancestor=$imageName")
                    if [ -z "\$CONTAINER_IDS" ]; then
                        echo "No running containers found for image: $imageName"
                        exit 0
                    fi
                    echo "Stopping containers..."
                    docker stop \$CONTAINER_IDS
                    echo "Removing containers..."
                    docker rm \$CONTAINER_IDS
                    echo "All containers from image $imageName have been stopped and removed."
                    """
                    docker.image('nodemain:v1.0').run('-d --expose 3000 -p 3000:3000')
                }
            }
        }

    }
}
