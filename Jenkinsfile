pipeline {
    agent any
    stages{
        stage('checkout'){
            steps {
                checkout([$class: 'GitSCM', 
                        branches: [[name: 'refs/heads/main']], 
                        userRemoteConfigs: [[url: 'https://github.com/Ji-noha/spring-petclinic-app.git']]])
            }
        }
        stage('Build and Test'){
            steps {
                bat '.\\mvnw.cmd clean install'
            }
        }
        // run sonarqube test 
        stage('Run SobarQube') {
            environment {
                scannerHome = tool 'spring_pet_tool'
            }
            steps {
                withSonarQubeEnv('spring_pet_server')
                bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=petclinic_1"
            }
        }
        
    }
}
