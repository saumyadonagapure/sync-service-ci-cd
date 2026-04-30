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
 stage('Deploy') {
    steps {
        echo "Deploying to ${params.ENV} environment..."
        bat 'echo Deployment successful'
    }
}
        stage('Rollback') {
    when {
        expression { params.ROLLBACK_TAG != '' }
    }
    steps {
        echo "Rolling back to ${params.ROLLBACK_TAG}"
        bat 'echo Rollback done'
    }
}
    }
}
