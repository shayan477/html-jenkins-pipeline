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
        
        stage('Test') {
            steps {
                echo 'Running validation tests...'
                sh '''
                    echo "Testing branch: $GIT_BRANCH"
                    if [ -f index.html ]; then
                        echo "✓ index.html found"
                    else
                        echo "✗ index.html not found"
                    fi
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying HTML files...'
                sh '''
                    mkdir -p ~/html-deploy/$GIT_BRANCH
                    cp -r * ~/html-deploy/$GIT_BRANCH/ || true
                    echo "✓ Deployed to: ~/html-deploy/$GIT_BRANCH"
                    ls -lh ~/html-deploy/$GIT_BRANCH
                '''
            }
        }
    }
    
    post {
        success {
            echo "✓ Pipeline completed successfully for branch: ${env.GIT_BRANCH}"
        }
        failure {
            echo "✗ Pipeline failed for branch: ${env.GIT_BRANCH}"
        }
        always {
            echo "Pipeline execution finished"
        }
    }
}