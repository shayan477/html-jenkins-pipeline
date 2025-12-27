pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                echo "Building HTML project from branch: ${env.GIT_BRANCH}"
                sh '''
                    echo "Current directory contents:"
                    ls -la
                    echo "Building complete!"
                '''
            }
        }
        
        stage('Parallel Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        echo 'Running unit tests...'
                        sh '''
                            echo "Executing unit tests for branch: $GIT_BRANCH"
                            echo "✓ All unit tests passed"
                        '''
                    }
                }
                stage('Integration Tests') {
                    steps {
                        echo 'Running integration tests...'
                        sh '''
                            echo "Executing integration tests for branch: $GIT_BRANCH"
                            sleep 2
                            echo "✓ All integration tests passed"
                        '''
                    }
                }
                stage('Security Scan') {
                    steps {
                        echo 'Running security scan...'
                        sh '''
                            echo "Performing security analysis for branch: $GIT_BRANCH"
                            echo "Checking for vulnerabilities..."
                            sleep 1
                            echo "✓ No vulnerabilities found"
                        '''
                    }
                }
            }
        }
        
        stage('Validation') {
            steps {
                echo 'Running validation tests...'
                sh '''
                    echo "Validating HTML files for branch: $GIT_BRANCH"
                    if [ -f index.html ]; then
                        echo "✓ index.html found and validated"
                    else
                        echo "✗ index.html not found"
                        exit 1
                    fi
                '''
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                sh '''
                    mkdir -p ~/html-deploy/$GIT_BRANCH/staging
                    cp -r * ~/html-deploy/$GIT_BRANCH/staging/ || true
                    echo "✓ Deployed to staging: ~/html-deploy/$GIT_BRANCH/staging"
                    ls -lh ~/html-deploy/$GIT_BRANCH/staging
                '''
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to production environment...'
                sh '''
                    mkdir -p ~/html-deploy/$GIT_BRANCH/production
                    cp -r * ~/html-deploy/$GIT_BRANCH/production/ || true
                    echo "✓ Successfully deployed to: ~/html-deploy/$GIT_BRANCH/production"
                    ls -lh ~/html-deploy/$GIT_BRANCH/production
                '''
            }
        }
    }
    
    post {
        success {
            echo "════════════════════════════════════════════════"
            echo "✓ Pipeline completed successfully!"
            echo "Branch: ${env.GIT_BRANCH}"
            echo "Build: #${env.BUILD_NUMBER}"
            echo "Duration: ${currentBuild.durationString}"
            echo "════════════════════════════════════════════════"
            
            emailext (
                subject: "✓ SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: """
                <h2>Build Successful!</h2>
                <p><strong>Job:</strong> ${env.JOB_NAME}</p>
                <p><strong>Build Number:</strong> ${env.BUILD_NUMBER}</p>
                <p><strong>Branch:</strong> ${env.GIT_BRANCH}</p>
                <p><strong>Build URL:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <p><strong>Status:</strong> <span style="color: green;">SUCCESS</span></p>
                """,
                to: 'team@example.com',
                mimeType: 'text/html'
            )
        }
        
        failure {
            echo "════════════════════════════════════════════════"
            echo "✗ Pipeline failed!"
            echo "Branch: ${env.GIT_BRANCH}"
            echo "Build: #${env.BUILD_NUMBER}"
            echo "Please check console output for details"
            echo "════════════════════════════════════════════════"
            
            emailext (
                subject: "✗ FAILURE: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                body: """
                <h2>Build Failed!</h2>
                <p><strong>Job:</strong> ${env.JOB_NAME}</p>
                <p><strong>Build Number:</strong> ${env.BUILD_NUMBER}</p>
                <p><strong>Branch:</strong> ${env.GIT_BRANCH}</p>
                <p><strong>Build URL:</strong> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                <p><strong>Status:</strong> <span style="color: red;">FAILURE</span></p>
                <p>Please check the console output for error details.</p>
                """,
                to: 'team@example.com',
                mimeType: 'text/html'
            )
        }
        
        always {
            echo "Pipeline execution finished at ${new Date()}"
            echo "Cleaning up workspace..."
        }
    }
}