pipeline {
    agent none

    stages {
        stage('Checkout & Build') {
            agent { label 'built-in' }  // checkout on Jenkins master
            steps {
                git branch: 'main',
                    credentialsId: 'jenkins',
                    url: 'git@github.com:gajanan-maid/todoapp-docker.git'
                sh 'docker build -t todoapp-docker .'
            }
        }

        stage('Deploy') {
            agent { label 'deploy-node' }  // deploy on your node
            steps {
                sh '''
                    docker stop todoapp || true
                    docker rm todoapp || true
                    docker run -d \
                        --name todoapp \
                        -p 80:80 \
                        --restart always \
                        todoapp-docker
                '''
            }
        }
    }
}
