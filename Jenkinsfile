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
                sh './mvnw clean package -DskipTests'
            }
        }
        
    }
}
