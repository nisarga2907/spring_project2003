
pipeline {
    agent any

    // Define parameters
    parameters {
        string(name: 'DOCKER_REPO', defaultValue: 'nisargasj2907/spring_project2003', description: 'Docker repository name')
        string(name: 'IMAGE_TAG', defaultValue: 'v1', description: 'Tag for Docker image')
        string(name: 'GIT_BRANCH', defaultValue: 'master', description: 'Git branch to build')
    }

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-cred')  // Docker Hub credentials ID
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${params.GIT_BRANCH}", url: 'https://github.com/your-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${params.DOCKER_REPO}:${params.IMAGE_TAG} ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh "echo ${DOCKER_HUB_CREDENTIALS_PSW} | docker login -u ${DOCKER_HUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${params.DOCKER_REPO}:${params.IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            sh "docker logout"
        }
    }
}
