pipeline {
    agent any
    
    environment {
    DOCKER_HUB_CREDENTIALS = 'dockerhub-credentials' // The ID you just created
    DOCKER_IMAGE = "chandancj7/react-jenkins-docker-k8s"
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
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                                      usernameVariable: 'DOCKER_USERNAME', 
                                                      passwordVariable: 'DOCKER_PASSWORD')]) {
                        docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                            docker.image(DOCKER_IMAGE).push("latest")
                        }
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the Kubernetes deployment and service configurations
                    sh "kubectl apply -f ~/k8s-deployment/k8s-deployment.yaml"
                    sh "kubectl apply -f ~/k8s-deployment/k8s-service.yaml"
                }
            }
        }
    }
}
