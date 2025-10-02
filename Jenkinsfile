pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
                archiveArtifacts artifacts: 'public/js/bundle.js'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Code Quality (SonarCloud)') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh """
                        sonar-scanner \
                          -Dsonar.projectKey=Tyler2811_8.2CDevSecOps \
                          -Dsonar.organization=tyler2811-1 \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=https://sonarcloud.io \
                          -Dsonar.login=$SONAR_TOKEN
                    """
                }
            }
        }

        stage('Security (npm audit)') {
            steps {
                sh 'npm audit || true'
            }
        }
    }
}
