pipeline {
    agent { label 'agent-node' } 

    environment {
        IMAGE_NAME = "sayantan2k21/demo-app"
        IMAGE_TAG = "v1.0-${BUILD_NUMBER}"
        CONTAINER_NAME = "demo-app-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Ashutosh-9216/flask-app-for-jenkins-docker-example.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                sh """
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
        stage('Health Check') {
            steps {
                sh "sleep 10 && curl localhost:5000"
            }
        }
    }

    post {
        always {
            echo "Job executed on agent: ${env.NODE_NAME}"
        }
    }
}
