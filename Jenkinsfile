pipeline {
    agent any

    tools {
        nodejs 'Node-18' // Make sure this is set in Jenkins -> Global Tools
    }

    environment {
        AWS_REGION = 'us-east-1'
        S3_BUCKET = 'yash-react-app '
        BUILD_DIR = 'build'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Upload to S3') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'aws-creds',
                        usernameVariable: 'AWS_ACCESS_KEY_ID',
                        passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                    )
                ]) {
                    sh '''
                        echo "Setting AWS credentials..."
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws configure set region $AWS_REGION

                        echo "Uploading to S3..."
                        aws s3 cp $BUILD_DIR s3://$S3_BUCKET/ --recursive
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment to S3 successful!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
