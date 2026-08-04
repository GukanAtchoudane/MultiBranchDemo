pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Running build for ${env.BRANCH_NAME}..."
                sh 'echo "npm install / mvn build here"'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'echo "npm test / mvn test here"'
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'develop'
            }
            steps {
                echo 'Deploying to STAGING (only runs on develop branch)'
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to PRODUCTION (only runs on main branch)'
            }
        }

        stage('Feature Branch Notice') {
            when {
                not {
                    anyOf {
                        branch 'main'
                        branch 'develop'
                    }
                }
            }
            steps {
                echo "This is a feature branch (${env.BRANCH_NAME}) — build & test only, no deploy."
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline succeeded on branch: ${env.BRANCH_NAME}"
        }
        failure {
            echo "❌ Pipeline failed on branch: ${env.BRANCH_NAME}"
        }
    }
}
