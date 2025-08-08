pipeline {
    agent any
    stages{
        stage('checkout'){
            steps {
                git 'https://github.com/Ji-noha/spring-petclinic-app.git'
            }
        }
        stage('Build'){
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }
        
    }
}
