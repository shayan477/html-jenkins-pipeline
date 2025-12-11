pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Approve Deployment') {
            steps {
                input message: 'Deploy to production?', 
                      ok: 'Deploy',
                      submitter: 'admin'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production...'
            }
        }

        stage('Parallel Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh 'mvn test'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        sh 'mvn integration-test'
                    }
                }
                stage('Security Scan') {
                    steps {
                        sh 'mvn dependency-check:check'
                    }
                }
            }
        }
    }

    // ✅ Notifications go here
    post {
        success {
            slackSend(
                color: 'good',
                message: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
        failure {
            slackSend(
                color: 'danger',
                message: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            )
        }
    }
}
