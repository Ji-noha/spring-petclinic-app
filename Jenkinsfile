pipeline {
    agent any
    stages{
        environment {
            SONAR_PROJEECT_KEY  = '11'
            SONAR_HOST_URL = 'http://localhost:9000'
            SONAR_LOGIN = credentials('sonar-token')
            scannerHome = tool 'spring_pet_tool'
        }  

        stage('checkout'){
            steps {
                checkout([$class: 'GitSCM', 
                        branches: [[name: 'refs/heads/main']], 
                        userRemoteConfigs: [[url: 'https://github.com/Ji-noha/spring-petclinic-app.git']]])
            }
        }

        stage('Start SonarQube') {
            steps {
                bat 'docker run -d --name sonarqube -p 9000:9000 sonarqube:10.3-community'
                bat 'timeout /t 30'
            }
        }

        stage('Build'){
            steps {
                bat '.\\mvnw.cmd clean package -DskipTests'
            }
        }
        
        stage('test'){
            steps {
                bat '.\\mvnw.cmd clean test '
            }
        }
        
        // run sonarqube test 
            stage('Run SonarQube') {
                    steps {
                            bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=%SONAR_POJECT_KEY% -Dsonar.host.url=%SONAR_HOST_URL% -Dsonar.login=%SONAR_LOGIN%"
                        
                    }
            }
    }

    post {
        always {
            bat 'docker stop sonarqube || exit 0'
            bat 'docker rm sonarqube || exit 0'    
        }
    }    
    
}
