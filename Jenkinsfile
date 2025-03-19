pipeline {
    agent any

    environment {
        IMAGE_NAME = "ali/cicd-example"
        NODE_PORT = "32000"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh "docker push $IMAGE_NAME"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                    kubectl apply -f k8s/deployment.yaml
                    kubectl apply -f k8s/service.yaml
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh "kubectl get pods"
                    sh "kubectl get services"
                }
            }
        }
    }
}
