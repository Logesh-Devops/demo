pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'logeshlogan'
        DOCKER_HUB_PASSWORD = 'dckr_pat_FkFmZwfQ2QTqLd0uxRKZNHjY_R4'
        REPO = 'logeshlogan/demo-vishal'
        GIT_REPO = 'https://github.com/Logesh-Devops/demo.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}"
            }
        }
        stage('Get Timestamp') {
            steps {
                script {
                    timestamp = new Date().format("yyyyMMddHHmmss")
                    echo "Build Timestamp: ${timestamp}"
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Use a valid image name format with a timestamp
                    imageName = "${REPO}:${timestamp}"
                    dockerImage = docker.build(imageName)
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    bat "echo ${DOCKER_HUB_PASSWORD} | sudo docker login -u ${DOCKER_HUB_USERNAME} --password-stdin https://index.docker.io/v1/"
                    dockerImage.push()
                    bat "docker logout"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
