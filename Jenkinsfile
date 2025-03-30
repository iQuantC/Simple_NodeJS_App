pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    environment {
        SONAR_PROJECT_KEY = 'node-app'
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {
        stage('Checkout Github') {
            steps {
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/iQuantC/Simple_NodeJS_App.git'
            }
        }

        stage('Install node dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('sonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'Sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://34.204.186.74:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs.'
        }
    }
}

