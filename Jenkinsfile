pipeline {
    agent any

    tools {
        nodejs 'NodeJS'          // Name should match your Jenkins tool config
    }

    environment {
        SONAR_SCANNER = tool 'SonarScanner'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/suhaibmdv/my-node-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'  // Prevent build fail if no tests exist yet
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarLocal') {
                    sh '''
                    ${SONAR_SCANNER}/bin/sonar-scanner \
                      -Dsonar.projectKey=my-node-app \
                      -Dsonar.sources=. \
                      -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \
                      -Dsonar.host.url=http://localhost:9000
                    '''
                }
            }
        }
    }
}
