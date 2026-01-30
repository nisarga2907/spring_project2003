pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_NAME', defaultValue: 'spring_project2003', description: 'Docker image name')
        string(name: 'IMAGE_TAG', defaultValue: 'v1', description: 'Docker image tag')
        string(name: 'DOCKERHUB_USERNAME', defaultValue: 'nisarga2907')

    }

    environment {
        IMAGE = "${params.DOCKERHUB_USERNAME}/${params.IMAGE_NAME}:${params.IMAGE_TAG}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/nisarga2907/spring_project2003.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE} .
                """
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                    echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                    """
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh """
                docker push ${IMAGE}
                """
            }
        }

        stage('Delete Local Image') {
            steps {
                sh """
                docker rmi ${IMAGE} || true
                """
            }
        }

        stage('Docker Logout') {
            steps {
                sh """
                docker logout
                """
            }
        }
    }
}
