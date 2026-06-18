pipeline {
    agent { label 'node1' }  // runs on your SSH node

    environment {
        IMAGE_NAME = 'todoapp-docker'
        CONTAINER_NAME = 'todoapp'
        APP_PORT = '80'
        CONTAINER_PORT = '80'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    credentialsId: 'jenkins', 
                    url: 'git@github.com:gajanan-maid/todoapp-docker.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker images | grep ${IMAGE_NAME}'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    # Stop and remove existing container if running
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true

                    # Run new container
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${APP_PORT}:${CONTAINER_PORT} \
                        --restart always \
                        ${IMAGE_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo "✅ App deployed successfully on node!"
        }
        failure {
            echo "❌ Deployment failed. Check logs above."
        }
    }
}
