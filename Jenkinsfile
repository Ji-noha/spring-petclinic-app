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
        
    }
}
