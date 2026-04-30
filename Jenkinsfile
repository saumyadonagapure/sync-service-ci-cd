Groovy

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
                sh 'echo Build successful'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'echo Tests passed'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "echo Pushing image..."
                }
            }
        }

        stage('Approval for Prod') {
            when {
                expression { params.ENV == 'prod' }
            }
            steps {
                input message: "Approve production deployment?"
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (params.ROLLBACK_TAG) {
                        sh "./scripts/rollback.sh ${params.ROLLBACK_TAG}"
                    } else {
                        sh "./scripts/deploy.sh ${IMAGE_NAME} ${TAG} ${params.ENV}"
                    }
                }
            }
        }

        stage('Smoke Test') {
            steps {
                sh "echo Checking health endpoint..."
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed!"
        }
    }
}