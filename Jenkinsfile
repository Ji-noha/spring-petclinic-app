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
        
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                        branches: [[name: 'refs/heads/main']],
                        userRemoteConfigs: [[url: 'https://github.com/Ji-noha/spring-petclinic-app.git']]])
            }
        }
        

        stage('Build') {
            steps {
                bat ".\\mvnw.cmd package -DskipTests"
            }
        }

        stage('Test') {
            steps {
                bat '.\\mvnw.cmd clean test'
            }
        }
        
        stage('Run SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]){
                    bat """
                    C:\\sonar-tool\\sonar-scanner-cli-7.2.0.5079-windows-x64.zip ^
                    -Dsonar.projectKey=%SONAR_PROJECT_KEY% ^
                    -Dsonar.sources=. ^
                    -Dsonar.java.binaries=target\\classes ^
                    -Dsonar.host.url=%SONAR_HOST_URL% ^
                    -Dsonar.login=%SONAR_TOKEN%
                    """
                }
            }
        }
        /*
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
        stage('Set Docker Context') {
            steps {
                bat 'docker context use default'
            }
        }

        stage('Push Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-cred', url: 'https://index.docker.io/v1/']) {
                   // bat "docker push %REGISTRY/%DOCKER_IMAGE%"
                    bat """
                        docker build -t noha04/pet-app:latest .
                        docker push noha04/pet-app:latest
                    """
                }
            }
        
        }
        /*
        stage('Deploy to Railway') {
            steps {
                withCredentials([string(credentialsId: 'railway-api-token', variable: 'RAILWAY_TOKEN')]) {
                    bat """
                        C:\\tools\\railway.cmd login --token %RAILWAY_TOKEN%
                        C:\\tools\\railway.cmd up --service %SERVICE_NAME% --detach
                    """
                }
            }
        }
        
        stage('Deploy to Railway') {
            steps {
                withCredentials([string(credentialsId: 'railway-api-token', variable: 'RAILWAY_TOKEN')]) {
                bat """
                    set RAILWAY_TOKEN=%RAILWAY_TOKEN%
                    railway whoami
                    railway up
                """
                }
            }
        }

    
    }
    */
}

}
