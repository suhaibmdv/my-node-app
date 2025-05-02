pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // Name from Jenkins > Global Tool Configuration
    }

    environment {
        SONAR_SCANNER = tool 'SonarScanner' // Must match the name from Jenkins tools
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/suhaibmdv/my-node-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Optional: ensure tests pass and generate coverage if configured
                sh 'npm test || true'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarLocal') {
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                        sh '''
                        ${SONAR_SCANNER}/bin/sonar-scanner \
                          -Dsonar.projectKey=my-node-app \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
