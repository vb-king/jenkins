pipeline {
    agent any

    // Parameters
    parameters {
        string(name: 'ENV', defaultValue: 'dev', description: 'Environment to deploy (dev/staging/prod)')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Whether to run unit tests')
        string(name: 'APP_NAME', defaultValue: 'MyApp', description: 'Application name')
    }

    // Environment variables
    environment {
        ENVIRONMENT = "${params.ENV}"
        APPLICATION = "${params.APP_NAME}"
        TESTS_ENABLED = "${params.RUN_TESTS}"
    }

    stages {

        // ===== Sequential Stages =====
        stage('Build') {
            steps {
                echo "🔧 Building ${env.APPLICATION} for environment: ${env.ENVIRONMENT}"
            }
        }

        stage('Test') {
            when {
                expression { return env.TESTS_ENABLED.toBoolean() }
            }
            steps {
                echo '✅ Running unit tests...'
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying ${env.APPLICATION} to ${env.ENVIRONMENT}"
            }
        }

        // ===== Parallel Stages =====
        stage('Parallel Tasks') {
            parallel {
                stage('Notify Team') {
                    steps {
                        echo '📢 Sending notification to the team...'
                    }
                }

                stage('Post-Deployment Checks') {
                    steps {
                        echo '🔍 Running post-deployment verification...'
                    }
                }

                stage('Backup') {
                    steps {
                        echo '💾 Taking backup of previous deployment...'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs for errors.'
        }
        always {
            echo '🔄 This always runs, regardless of success or failure.'
        }
    }
}
