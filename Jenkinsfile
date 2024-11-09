pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub_credentials')
        DOCKER_IMAGE = "chandancj7/react-jenkins-docker-k8s"
        K8S_CREDENTIALS = credentials('k8s_jenkins_token') // Kubernetes credentials ID
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/CHANDANG7/react-jenkins-docker-k8s.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub_credentials') {
                        docker.image(DOCKER_IMAGE).push("latest")
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'k8s_jenkins_token', variable: 'KUBECONFIG')]) {
                        sh "kubectl apply -f k8s-deployment.yaml"
                        sh "kubectl apply -f k8s-service.yaml"
                    }
                }
            }
        }
    }
}
