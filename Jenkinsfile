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
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat """
                        docker build -t ${DOCKER_HUB_REPO}:latest .
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKER_HUB_REPO}:latest
                    """
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
