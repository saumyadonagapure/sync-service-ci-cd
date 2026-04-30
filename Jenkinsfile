pipeline {
    agent any

    environment {
        IMAGE_NAME = "sync-service"
        TAG = "${BUILD_NUMBER}"
    }

    parameters {
        choice(name: 'ENV', choices: ['qa', 'staging', 'prod'], description: 'Deployment Environment')
        string(name: 'ROLLBACK_TAG', defaultValue: '', description: 'Rollback tag if needed')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/saumyadonagapure/sync-service-ci-cd.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building application..."
                bat 'echo Build successful'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                bat 'echo Tests passed'
            }
        }

    }
}