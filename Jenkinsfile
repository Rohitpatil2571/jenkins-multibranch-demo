pipeline {
    agent { label 'docker-agent' }

    environment {
        APP_NAME = "jenkins-multibranch-demo"
        ACR_LOGIN_SERVER = "jenkinslabacrbr9lbn.azurecr.io"
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
        stage('Docker Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'acr-credentials',
                        usernameVariable: 'ACR_USERNAME',
                        passwordVariable: 'ACR_PASSWORD'
                    )
                ]) {
                    sh '''
                        docker tag $APP_NAME:$BUILD_NUMBER $ACR_LOGIN_SERVER/$APP_NAME:$BRANCH_NAME-$BUILD_NUMBER
                        docker login $ACR_LOGIN_SERVER -u $ACR_USERNAME -p $ACR_PASSWORD
                        docker push $ACR_LOGIN_SERVER/$APP_NAME:$BRANCH_NAME-$BUILD_NUMBER
                    '''
                }
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