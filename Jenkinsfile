pipeline {
    agent any

    environment {
        REGISTRY = credentials('docker_registry')
        DOCKER_IMAGE = credentials('docker_image')
    }

    stages {
        
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                        branches: [[name: 'refs/heads/main']],
                        userRemoteConfigs: [[url: 'https://github.com/Ji-noha/spring-petclinic-app.git']]])
            }
        }
        

        stage('Build') {
            steps {
                bat ".\\mvnw.cmd package -DskipTests"
            }
        }

        stage('Test') {
            steps {
                bat '.\\mvnw.cmd clean test'
            }
        }

        stage('Set Docker Context') {
            steps {
                bat 'docker context use default'
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-cred', url: 'https://index.docker.io/v1/']) {
                   // bat "docker push %REGISTRY/%DOCKER_IMAGE%"
                    bat """
                        docker build -t noha04/pet-app:latest .
                        docker push noha04/pet-app:latest
                    """
                }
            }
        
        }
    }

}
