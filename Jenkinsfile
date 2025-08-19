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
                bat '.\\mvnw.cmd package -DskipTests -Dcheckstyle.skip=true -U'
            }
        }

        stage('Test') {
            steps {
                bat '.\\mvnw.cmd clean test'
            }
        }
        /*
        stage('Run SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]){
                    bat "\"${scannerHome}\\bin\\sonar-scanner.bat\" -Dsonar.projectKey=%SONAR_PROJECT_KEY% -Dsonar.host.url=%SONAR_HOST_URL% -Dsonar.token=%SONAR_TOKEN% -Dsonar.java.binaries=target/classes"
                }
            }
        }
        //
        stage('Run SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]) {
                        bat """
                        docker run --rm -e SONAR_HOST_URL=http://127.0.0.1:9000 -e SONAR_LOGIN=%SONAR_TOKEN% -v "%cd%:/usr/src" sonarsource/sonar-scanner-cli ^
                            -Dsonar.projectKey=%SONAR_PROJECT_KEY% ^
                            -Dsonar.sources=. ^
                            -Dsonar.java.binaries=target\\classes
                        """
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // Inject SonarQube environment variables automatically
                withSonarQubeEnv('spring_pet_server') {
                    bat 'sonar-scanner'
                }
            }
        } 
        
        stage('SonarQube Analysis') {
            steps {
                bat '''
                docker run --rm -v "%cd%:/usr/src" -e SONAR_HOST_URL=http://192.168.8.169:9000 -e SONAR_LOGIN=sonar_token sonarsource/sonar-scanner-cli ^
                -Dsonar.projectKey=pet_ ^
                -Dsonar.sources=. ^
                -Dsonar.java.binaries=target/classes
                '''
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    docker.image('sonarsource/sonar-scanner-cli').inside {
                        bat """
                            sonar-scanner ^
                            -Dsonar.projectKey=YOUR_PROJECT_KEY ^
                            -Dsonar.sources=. ^
                            -Dsonar.java.binaries=target/classes ^
                            -Dsonar.host.url=http://192.168.8.169:9000 ^
                            -Dsonar.login=YOUR_SONAR_TOKEN ^
                            -Dcheckstyle.skip=true
                        """
                    }
                }
            }
        }
        */

        stage('Push Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-cred', url: 'https://index.docker.io/v1/']) {
                   // bat "docker push %REGISTRY/%DOCKER_IMAGE%"
                    bat """
                        docker push ${REGISTRY}/${DOCKER_IMAGE}
                    """
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
