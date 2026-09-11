pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-nginx'
        CONTAINER_NAME = 'nginx-container'
        HOST_PORT = '8085'
        CONTAINER_PORT = '80'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/prasannacn08/Nginx-application-1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run Nginx Container') {
            steps {
                sh '''
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${HOST_PORT}:${CONTAINER_PORT} \
                    ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                    sleep 3
                    curl http://localhost:${HOST_PORT}
                '''
            }
        }
    }

    post {
        success {
            echo "Nginx deployment successful!"
            echo "Access application at http://3.249.72.167:8085"
        }

        failure {
            echo "Nginx deployment failed!"
        }
    }
}
