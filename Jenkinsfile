pipeline {
    agent { label 'docker-agent' }

    environment {
        APP_NAME = "jenkins-multibranch-demo"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building application"
                echo "Branch: ${env.BRANCH_NAME}"
            }
        }
        stage('Test') {
            steps {
                echo "Running tests"
            }
        }
        stage('Docker Build') {
            steps {
                sh """
                    docker build -t ${APP_NAME}:${BUILD_NUMBER} .
                """
            }
        }
        stage('Deploy DEV') {
            when { branch 'dev' }
            steps { echo "Deploying to DEV environment" }
        }
        stage('Deploy UAT') {
            when { branch 'uat' }
            steps { echo "Deploying to UAT environment" }
        }
        stage('Deploy PROD') {
            when { branch 'prod' }
            steps { echo "Deploying to PROD environment" }
        }
    }
}
