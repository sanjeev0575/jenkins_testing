pipeline {
    agent any

    // ✅ 1. PARAMETERS (Dynamic Builds)
    parameters {
        string(name: 'APP_VERSION', defaultValue: '1.0.0', description: 'Application version to deploy')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'production'], description: 'Select deployment environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Do you want to run tests?')
    }

    // ✅ 2. ENVIRONMENT VARIABLES
    environment {
        APP_NAME    = 'my-jenkins-app'
        DEVELOPER   = 'Sanjeev'
        BUILD_DATE  = "${new Date().format('yyyy-MM-dd')}"
        DEPLOY_PATH = "/var/www/${params.ENVIRONMENT}"
    }

    triggers {
        githubPush()
    }

    stages {

        // ✅ STAGE 1 - Print all env variables & parameters
        stage('Initialize') {
            steps {
                echo "========================================="
                echo "App Name    : ${env.APP_NAME}"
                echo "Developer   : ${env.DEVELOPER}"
                echo "Build Date  : ${env.BUILD_DATE}"
                echo "Version     : ${params.APP_VERSION}"
                echo "Environment : ${params.ENVIRONMENT}"
                echo "Run Tests   : ${params.RUN_TESTS}"
                echo "Deploy Path : ${env.DEPLOY_PATH}"
                echo "========================================="
            }
        }

        // ✅ STAGE 2 - Build
        stage('Build') {
            steps {
                echo "Building ${env.APP_NAME} version ${params.APP_VERSION}..."
                echo "Build Number : ${env.BUILD_NUMBER}"
                echo "Build URL    : ${env.BUILD_URL}"
                echo "Workspace    : ${env.WORKSPACE}"
                echo "Build complete ✅"
            }
        }

        // ✅ STAGE 3 - PARALLEL BUILDS (Test stage runs in parallel)
        stage('Test') {
            when {
                expression { return params.RUN_TESTS == true }
            }
            parallel {

                stage('Unit Test') {
                    steps {
                        echo "Running Unit Tests..."
                        sleep(time: 3, unit: 'SECONDS')
                        echo "Unit Tests Passed ✅"
                    }
                }

                stage('Integration Test') {
                    steps {
                        echo "Running Integration Tests..."
                        sleep(time: 5, unit: 'SECONDS')
                        echo "Integration Tests Passed ✅"
                    }
                }

                stage('Security Scan') {
                    steps {
                        echo "Running Security Scan..."
                        sleep(time: 2, unit: 'SECONDS')
                        echo "Security Scan Passed ✅"
                    }
                }

            }
        }

        // ✅ STAGE 4 - Deploy based on Environment parameter
        stage('Deploy') {
            steps {
                script {
                    if (params.ENVIRONMENT == 'production') {
                        echo "🚀 Deploying to PRODUCTION - Extra caution!"
                        echo "Deploying ${env.APP_NAME} v${params.APP_VERSION} to ${env.DEPLOY_PATH}"
                    } else if (params.ENVIRONMENT == 'staging') {
                        echo "🚀 Deploying to STAGING..."
                        echo "Deploying ${env.APP_NAME} v${params.APP_VERSION} to ${env.DEPLOY_PATH}"
                    } else {
                        echo "🚀 Deploying to DEV..."
                        echo "Deploying ${env.APP_NAME} v${params.APP_VERSION} to ${env.DEPLOY_PATH}"
                    }
                }
            }
        }

    }

    // ✅ POST BUILD ACTIONS
    post {
        success {
            echo "✅ Pipeline SUCCESS - ${env.APP_NAME} v${params.APP_VERSION} deployed to ${params.ENVIRONMENT}"
        }
        failure {
            echo "❌ Pipeline FAILED - Check logs for errors"
        }
        always {
            echo "Pipeline finished - Build #${env.BUILD_NUMBER}"
        }
    }
}
