pipeline {
    agent any

    environment {
        IMAGE_NAME = 'vijay4devops/argocd-demo'
        GIT_REPO = 'https://github.com/vijay2181/kubernetescode.git'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${env.BUILD_NUMBER} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub', url: '']) {
                    sh "docker push ${IMAGE_NAME}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}
