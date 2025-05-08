pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')  /
        IMAGE_NAME = 'chari9515/nodejs-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch:'main',url:'https://github.com/your-username/your-nodejs-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy Container (optional)') {
            steps {
                sh '''
                docker stop nodejs-app || true
                docker rm nodejs-app || true
                docker run -d --name nodejs-app -p 3000:3000 ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo "Build and deployment successful!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
