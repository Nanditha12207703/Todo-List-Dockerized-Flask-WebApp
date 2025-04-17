pipeline {
    agent any

    environment {
        IMAGE_NAME = 'todo-flask-app'
        CONTAINER_NAME = 'todo-flask-container'
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo "Cloning the GitHub repo..."
                // Repo is auto-cloned when using 'Pipeline script from SCM'
                sh 'ls -l'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Stop Previous Container') {
            steps {
                script {
                    echo "Stopping any running container with the same name..."
                    sh """
                        if [ \$(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                            docker stop ${CONTAINER_NAME}
                            docker rm ${CONTAINER_NAME}
                        fi
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running new Docker container..."
                sh "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo '✅ Deployment complete! Visit http://localhost:5000'
        }
        failure {
            echo '❌ Something went wrong.'
        }
    }
}
