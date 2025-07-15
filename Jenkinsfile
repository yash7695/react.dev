pipeline {
    agent any

    environment {
        AWS_REGION  = 'us-east-1'
        S3_BUCKET   = 'yash-react-app'          // 🔁 Replace with actual bucket name
    }

        stage('Install Dependencies') {
            steps {
                sh '/usr/local/bin/npm install --legacy-peer-deps'
            }
        }


        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Upload to S3') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-credentials-id') {
                    sh '''
                        aws s3 sync build/ s3://$S3_BUCKET/ --delete
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
