pipeline {

    agent { label 'docker-agent' }

    environment {
        REGISTRY = "myacr1234.azurecr.io"
        IMAGE = "myapp"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REGISTRY/$IMAGE:v1 .'
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                az acr login --name myacr1234
                docker push $REGISTRY/$IMAGE:v1
                '''
            }
        }

        stage('Deploy to AKS') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
