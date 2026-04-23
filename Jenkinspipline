pipeline {
    agent any 

    environment {
        APP_NAME = 'my-app'
        DEPLOY_ENV = 'staging'
    }

    stages {
        stage('Build') {
            steps {
                echo "Building ${APP_NAME}..."
                // Example shell command to build
                sh 'make build' 
            }
        }

        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'make test-unit'
                    }
                }
                stage('Linting') {
                    steps {
                        sh 'make lint'
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main' // Only deploy if on the main branch
            }
            steps {
                echo "Deploying to ${DEPLOY_ENV}..."
                sh 'make publish'
            }
        }
    }

    post {
        always {
            echo 'Archiving artifacts...'
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }
        failure {
            echo 'The pipeline failed. Sending notification...'
        }
    }
}
