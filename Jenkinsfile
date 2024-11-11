pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://localhost:9000'
        SONARQUBE_TOKEN = credentials('sqp_42a51d169ac3371b8e82f30d7c3a3c7046e8511b')
    }

    stages {
        stage('Checkout') {
            steps {
                // Descargar el código desde el repositorio
                checkout scm
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=DVWA \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=$SONARQUBE_URL \
                    -Dsonar.login=$SONARQUBE_TOKEN
                    '''
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Skipping build step as DVWA is a PHP application'
            }
        }
        
        stage('Deploy to Local Environment') {
            steps {
                // Copiar archivos al directorio de DVWA en la máquina local
                sh '''
                cp -r * /var/www/html/DVWA
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
