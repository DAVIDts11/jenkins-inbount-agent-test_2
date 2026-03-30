pipeline {
    agent { label 'dind-agent' }
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_CRED')
    }
    
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        
        stage('Pull NGINX Image') {
            steps {
                echo "Pulling NGINX image..."
                sh 'docker pull nginx:latest'
            }
        }

        stage('Run NGINX Container') {
            steps {
                echo "Running NGINX on port 8001..."
                // Stop/remove existing container if exists
                sh '''
                if [ $(docker ps -aq -f name=nginx-test) ]; then
                    docker rm -f nginx-test
                fi
                docker run -dit --name nginx-test -p 8001:80 nginx:latest
                '''
            }
        }

        stage('Check NGINX Status') {
            steps {
                sh 'docker ps -a | grep nginx-test'
            }
        }
        stage('login dockerhub') {
            steps {
                sh 'echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login --username ${DOCKERHUB_CREDENTIALS_USR} --password-stdin'
            }
        }
    }
}
