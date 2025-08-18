pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = credentials('pet_')
        SONAR_HOST_URL = credentials('sonar_host_url')
        scannerHome = credentials('scanner_home')
        REGISTRY = credentials('docker_registry')
        DOCKER_IMAGE = credentials('docker_image')
        RAILWAY_TOKEN = credentials('railway-api-token')
        SERVICE_NAME = credentials('service_name')
    }

    stages {
        /*
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                        branches: [[name: 'refs/heads/main']],
                        userRemoteConfigs: [[url: 'https://github.com/Ji-noha/spring-petclinic-app.git']]])
            }
        }
        */

        stage('Build') {
            steps {
                bat '.\\mvnw.cmd package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat '.\\mvnw.cmd clean test'
            }
        }

        stage('Run SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'final-token', variable: 'SONAR_TOKEN')]){
                    bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=%SONAR_PROJECT_KEY% -Dsonar.host.url=%SONAR_HOST_URL% -Dsonar.token=%SONAR_TOKEN% -Dsonar.java.binaries=target/classes"
                }
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-cred', url: "https://$REGISTRY"]) {
                    bat "docker push %REGISTRY/%DOCKER_IMAGE%"
                }
            }
        
        }

        stage('Deploy to Railway') {
            steps {
                withCredentials([string(credentialsId: 'railway-api-token', variable: 'RAILWAY_TOKEN')]) {
                    bat """
                        railway login --token %RAILWAY_TOKEN%
                        railway up --service %SERVICE_NAME% --detach
                    """
                }
            }
        }

    
    }

}
