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
                    sh "echo ${DOCKER_HUB_PASSWORD} | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin"
                    dockerImage.push()
                    sh "docker logout"
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
