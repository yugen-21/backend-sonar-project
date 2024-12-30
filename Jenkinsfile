pipeline {
    agent any
    tools {
        nodejs 'nodejs' 
    }
 
    environment {
        NODEJS_HOME = 'C://Program Files//nodejs'
        SONAR_SCANNER_PATH = 'C://Users//shama//Downloads//sonar-scanner-cli-6.2.1.4610-windows-x64//sonar-scanner-6.2.1.4610-windows-x64//bin'
        PROJECT_DIR = 'backend/' // Global variable for the backend folder
    }
 
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
 
        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                cd %PROJECT_DIR%
                npm install || exit /b 1
                '''
            }
        }
 
        stage('Lint') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                cd %PROJECT_DIR%
                npm run lint || exit /b 1
                '''
            }
        }
 
        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                cd %PROJECT_DIR%
                npm run build || exit /b 1
                '''
            }
        }
 
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                cd %PROJECT_DIR%
                where sonar-scanner || exit /b 1
                sonar-scanner -Dsonar.projectKey=backend-project ^ 
                    -Dsonar.sources=. ^ 
                    -Dsonar.host.url=http://localhost:9000 ^ 
                    -Dsonar.token=%SONAR_TOKEN% || exit /b 1
                '''
            }
        }
    }
 
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
