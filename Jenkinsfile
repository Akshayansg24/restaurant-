pipeline {
    agent any

    environment {
        IMAGE_NAME = 'react-restaurant-app'
        DOCKER_HUB_REPO = 'akshayan24/restaurant-app'
    }

    stages {
        stage('Clone') {
            steps {

                git 'https://github.com/Akshayansg24/restaurant-'
            }
        }

        stage('Build React') {
            steps {
                bat 'npm install'
                bat 'npm run build'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub') {
                        def img = docker.build("${DOCKER_HUB_REPO}:latest")
                        img.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-id', variable: 'KUBECONFIG')]) {
                    bat '''
                    kubectl apply -f k8s-Deployment.yaml --validate=false
                    if %errorlevel% neq 0 exit /b 0
                    '''
                }
            }
        }
    }
}
