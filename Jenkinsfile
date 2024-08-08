pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'a98e93cd-05d4-4deb-8e0b-1e28f831bdb8'
        REPO = 'logeshlogan/demo-vishal'
        GIT_REPO = 'https://github.com/Logesh-Devops/demo.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}"
            }
        }
        stage('Get Commit ID') {
            steps {
                script {
                    latestCommitId = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                    echo "Latest Commit ID: ${latestCommitId}"
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${REPO}:${latestCommitId}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push()
                    }
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push('latest')
                    }
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
