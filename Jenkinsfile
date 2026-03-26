pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "dulevnikola/kiii-jenkins"
        DOCKER_REGISTRY = "docker.io"
        DOCKER_CREDENTIALS_ID = "dockerhub"
        GIT_CREDENTIALS_ID = "github"
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/dulev23/jenkins-pipeline-homework',
                    credentialsId: "${GIT_CREDENTIALS_ID}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${DOCKER_IMAGE}"
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo "Logging in and pushing to Docker Hub..."
                    withCredentials([usernamePassword(
                        credentialsId: "${DOCKER_CREDENTIALS_ID}",
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        sh """
                            echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin
                            docker push ${DOCKER_IMAGE}
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully! 🎉"
        }
        failure {
            echo "Pipeline failed! ❌"
        }
    }
}