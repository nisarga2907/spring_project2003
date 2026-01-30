pipeline {
    agent any

    // Parameters to customize each build
    parameters {
        string(name: 'DOCKER_REPO', defaultValue: 'nisargasj2907/spring_project2003', description: 'Docker repository name')
        string(name: 'IMAGE_TAG', defaultValue: 'v1', description: 'Tag for Docker image')
        string(name: 'GIT_BRANCH', defaultValue: 'main', description: 'Git branch to build')
    }

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-cred')  // Your Jenkins Docker Hub credentials ID
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Checking out branch ${params.GIT_BRANCH}"
                git branch: "${params.GIT_BRANCH}", url: 'https://github.com/nisarga/spring_project2003.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image ${params.DOCKER_REPO}:${params.IMAGE_TAG}"
                    sh "docker build -t ${params.DOCKER_REPO}:${params.IMAGE_TAG} ."
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    echo "Logging in to Docker Hub"
                    sh "echo ${DOCKER_HUB_CREDENTIALS_PSW} | docker login -u ${DOCKER_HUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo "Pushing Docker image to Docker Hub"
                    sh "docker push ${params.DOCKER_REPO}:${params.IMAGE_TAG}"
                }
            }
        }

        stage('Delete Local Docker Image') {
            steps {
                script {
                    echo "Deleting local Docker image ${params.DOCKER_REPO}:${params.IMAGE_TAG}"
                    sh "docker rmi ${params.DOCKER_REPO}:${params.IMAGE_TAG} || true"
                }
            }
        }

    }

    post {
        always {
            script {
                echo "Logging out from Docker Hub"
                sh "docker logout"
            }
        }
    }
}
