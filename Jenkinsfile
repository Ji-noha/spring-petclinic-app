pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = '11'
        SONAR_HOST_URL = 'http://localhost:9000'
        scannerHome = tool 'spring_pet_tool'
        SONAR_TOKEN='pet_token'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                        branches: [[name: 'refs/heads/main']],
                        userRemoteConfigs: [[url: 'https://github.com/Ji-noha/spring-petclinic-app.git']]])
            }
        }

        stage('Start SonarQube') {
            steps {
                bat 'docker run -d --name sonarqube10 -p 9000:9000 sonarqube'
                sleep 30
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
                withCredentials([string(credentialsId: '1', variable: 'SONAR_TOKEN')]){
                    bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=%SONAR_PROJECT_KEY% -Dsonar.host.url=%SONAR_HOST_URL% -Dsonar.token=%SONAR_TOKEN%"
                }
            }
        }
        stage('Cleanup SonarQube') {
            steps {
                bat 'docker stop sonarqube || exit 0'
                bat 'docker rm sonarqube || exit 0'
            }
            when {
                expression { true }
            }
        }
    }

    
    
    
}
