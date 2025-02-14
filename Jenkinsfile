pipeline {
    agent any

    environment {
        IMAGE_NAME = 'vijay1447/argocd-demo'
        GIT_REPO = 'https://github.com/vijay2181/kubernetescode.git'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${env.BUILD_NUMBER} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh "docker push ${IMAGE_NAME}:${env.BUILD_NUMBER}"
            }
        }

        stage('Trigger Second Job') {
        steps {
            script {
                    build job: 'kubernetesmanifest', parameters: [
                        string(name: 'DOCKERTAG', value: "${env.BUILD_NUMBER}")
                    ]
                }
            }
         }
    }
}
