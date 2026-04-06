pipeline {
    agent any

    tools {
        jdk 'jdk11'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jaiswaladi246/Petclinic.git'
            }
        }

        stage('Compile') {
            steps {
                bat '''
                mvn clean compile ^
                -Djavax.net.ssl.trustStoreType=Windows-ROOT
                '''
            }
        }

        stage('Test Cases') {
            steps {
                bat '''
                mvn test ^
                -Djavax.net.ssl.trustStoreType=Windows-ROOT
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    bat '''
                    "%SCANNER_HOME%\\bin\\sonar-scanner.bat" ^
                    -Dsonar.projectKey=Petclinic ^
                    -Dsonar.projectName=Petclinic ^
                    -Dsonar.java.binaries=target
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                bat '''
                mvn clean install ^
                -Djavax.net.ssl.trustStoreType=Windows-ROOT
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build, Test, and SonarQube Analysis completed successfully'
        }
        failure {
            echo '❌ Pipeline failed — check logs above'
        }
    }
}
