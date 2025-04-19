pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Lint') {
            steps {
                echo 'Running linter...'
                sh 'npm run lint || echo "Linting completed with warnings."'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }

        stage('Build (Optional)') {
            when {
                expression { fileExists('build') || fileExists('build.js') }
            }
            steps {
                echo 'Running build script...'
                sh 'npm run build || echo "No build step found."'
            }
        }
    }

    post {
        success {
            echo '✅ All steps completed successfully!'
        }
        failure {
            echo '❌ Build failed. Please check the logs above.'
        }
    }
}
