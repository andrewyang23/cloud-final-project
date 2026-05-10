pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-2'
        S3_BUCKET = 'andrew-cloud-project'
        CLOUDFRONT_DISTRIBUTION_ID = 'E2DEWR6BTZHCUT'
    }

    stages {
        stage('Checkout Code from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/andrewyang23/cloud-final-project.git'
            }
        }

        stage('Deploy Website to S3') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    sh 'aws s3 sync . s3://${S3_BUCKET} --delete'
                }
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${AWS_REGION}") {
                    sh 'aws cloudfront create-invalidation --distribution-id ${CLOUDFRONT_DISTRIBUTION_ID} --paths "/*"'
                }
            }
        }
    }

    post {
        success {
            echo 'Website successfully deployed to S3 and CloudFront cache invalidated.'
        }
        failure {
            echo 'Deployment failed. Check Jenkins logs for details.'
        }
    }
}
