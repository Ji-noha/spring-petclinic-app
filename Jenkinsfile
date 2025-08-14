pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = 'pet_'
        SONAR_HOST_URL = 'http://localhost:9000'
        scannerHome = tool 'spring_pet_tool'
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
        
    }

    
    
    
}
