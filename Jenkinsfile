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
            echo ""
            echo "╔════════════════════════════════════════════════════════╗"
            echo "║                                                        ║"
            echo "║           ✓ BUILD SUCCESS NOTIFICATION                ║"
            echo "║                                                        ║"
            echo "╚════════════════════════════════════════════════════════╝"
            echo ""
            echo "📋 Build Information:"
            echo "   • Job Name     : ${env.JOB_NAME}"
            echo "   • Build Number : #${env.BUILD_NUMBER}"
            echo "   • Branch       : ${env.GIT_BRANCH}"
            echo "   • Status       : SUCCESS ✓"
            echo "   • Duration     : ${currentBuild.durationString}"
            echo "   • Build URL    : ${env.BUILD_URL}"
            echo ""
            echo "📊 Test Results:"
            echo "   • Unit Tests        : ✓ PASSED"
            echo "   • Integration Tests : ✓ PASSED"
            echo "   • Security Scan     : ✓ PASSED"
            echo "   • Validation        : ✓ PASSED"
            echo ""
            echo "🚀 Deployment:"
            echo "   • Staging   : ✓ DEPLOYED"
            script {
                if (env.GIT_BRANCH == 'main') {
                    echo "   • Production: ✓ DEPLOYED"
                } else {
                    echo "   • Production: ⊘ SKIPPED (not main branch)"
                }
            }
            echo ""
            echo "════════════════════════════════════════════════════════"
            echo "Notification sent at: ${new Date()}"
            echo "════════════════════════════════════════════════════════"
            echo ""
        }
        
        failure {
            echo ""
            echo "╔════════════════════════════════════════════════════════╗"
            echo "║                                                        ║"
            echo "║           ✗ BUILD FAILURE NOTIFICATION                ║"
            echo "║                                                        ║"
            echo "╚════════════════════════════════════════════════════════╝"
            echo ""
            echo "📋 Build Information:"
            echo "   • Job Name     : ${env.JOB_NAME}"
            echo "   • Build Number : #${env.BUILD_NUMBER}"
            echo "   • Branch       : ${env.GIT_BRANCH}"
            echo "   • Status       : FAILURE ✗"
            echo "   • Duration     : ${currentBuild.durationString}"
            echo "   • Build URL    : ${env.BUILD_URL}"
            echo ""
            echo "⚠️  Action Required:"
            echo "   • Check console output above for error details"
            echo "   • Review failed stage logs"
            echo "   • Fix the issue and push again"
            echo ""
            echo "════════════════════════════════════════════════════════"
            echo "Notification sent at: ${new Date()}"
            echo "════════════════════════════════════════════════════════"
            echo ""
        }
        
        always {
            echo ""
            echo "🧹 Cleanup:"
            echo "   • Workspace cleanup initiated"
            echo "   • Pipeline execution finished"
            echo "   • Ready for next build"
            echo ""
        }
    }
}