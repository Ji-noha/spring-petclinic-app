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
        stage('Build'){
            steps {
                bat '.\\mvnw.cmd clean package -DskipTests'
            }
        }
        /*
        stage('test'){
            steps {
                bat '.\\mvnw.cmd test -Dspring.profiles.active=mysql'
            }
        }
        */
        // run sonarqube test 
            stage('Run SonarQube') {
                    environment {
                        scannerHome = tool 'spring_pet_tool'
                    }
                    steps {
                        withSonarQubeEnv('spring_pet_server') {
                            bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=11"
                        }
                    }
            }

    }
        
    
}
