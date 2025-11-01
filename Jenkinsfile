pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "devops-webapp"
        DOCKERHUB_USER = "<your-dockerhub-username>"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/<your-username>/devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKERHUB_PASS')]) {
                    sh '''
                    echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
                    docker tag $DOCKER_IMAGE $DOCKERHUB_USER/$DOCKER_IMAGE:latest
                    docker push $DOCKERHUB_USER/$DOCKER_IMAGE:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f k8s-deployment.yaml
                kubectl apply -f k8s-service.yaml
                '''
            }
        }
    }
}
