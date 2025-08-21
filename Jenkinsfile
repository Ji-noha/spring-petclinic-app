pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = credentials('sonar_project_key')
        SONAR_HOST_URL = credentials('sonar_host_url')
        scannerHome = credentials('scanner_home')
        REGISTRY = credentials('docker_registry')
        DOCKER_IMAGE = credentials('docker_image')
        RAILWAY_TOKEN = credentials('railway-api-token')
        SERVICE_NAME = credentials('service_name')
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
        /*
        stage('Deploy to Railway') {
            steps {
                withCredentials([string(credentialsId: 'railway-api-token', variable: 'RAILWAY_TOKEN')]) {
                    bat """
                        C:\\tools\\railway.cmd login --token %RAILWAY_TOKEN%
                        C:\\tools\\railway.cmd up --service %SERVICE_NAME% --detach
                    """
                }
            }
        }
        
        stage('Deploy to Railway') {
            steps {
                withCredentials([string(credentialsId: 'railway-api-token', variable: 'RAILWAY_TOKEN')]) {
                bat """
                    set RAILWAY_TOKEN=%RAILWAY_TOKEN%
                    railway whoami
                    railway up
                """
                }
            }
        }

    
    }
    */
        stage('Deploy') {
            steps {
                bat 'docker-compose up -d'
            }
        }
    }

}
