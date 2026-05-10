pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-2'
        S3_BUCKET = 'andrew-cloud-project'
        CLOUDFRONT_DISTRIBUTION_ID = 'E2DEWR6BTZHCUT'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/andrewyang23/cloud-final-project.git'
            }
        }

        stage('Deploy to S3') {
            steps {
                sh 'aws s3 sync . s3://${S3_BUCKET} --delete'
            }
        }

        stage('Invalidate CloudFront') {
            steps {
                sh 'aws cloudfront create-invalidation --distribution-id ${CLOUDFRONT_DISTRIBUTION_ID} --paths "/*"'
            }
        }
    }
}
