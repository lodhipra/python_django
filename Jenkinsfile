pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "lodhipr/python-django:latest"
    }

    stages {
        stage('Cloning GitHub Repo') {
            steps {
                git "https://github.com/lodhipra/python_django.git"
            }
        }

        stage('Building Docker image') {
            steps {
                sh"docker build -t ${DOCKER_IMAGE} ."
            }
        }
        stage('Pushing Docker image on DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'password', usernameVariable: 'username')]) {
                    sh"docker login --username ${username} --password ${password}"
                }
                sh"docker push ${DOCKER_IMAGE}"
            }
        }
        stage('Deploying Application') {
            steps {
                sh"kubectl apply -f deployment.yaml"
            }
        }
        stage('Creating Service') {
            steps {
                sh"kubectl apply -f service.yaml"
            }
        }
        stage('Exposing minikube ip with port') {
            steps {
                sh "minikube service python-django-service --url"
            }
        }
    }
}
