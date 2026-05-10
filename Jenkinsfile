pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AKIAXNLQP53GUCLIGNMR')
        AWS_SECRET_ACCESS_KEY = credentials('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')
        CLOUDFRONT_DISTRIBUTION_ID = 'E2DEWR6BTZHCUT'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/cloud-final-project.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building static website...'
                // No build tools needed for static sites, just confirmation
            }
        }

        stage('Deploy to S3') {
            steps {
                echo 'Deploying to S3 bucket...'
                sh 'aws s3 sync . s3://andrew-cloud-project --delete'
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                echo 'Invalidating CloudFront cache...'
                sh 'aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_DISTRIBUTION_ID --paths "/*"'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful! Your website is live and secure.'
        }
        failure {
            echo 'Deployment failed. Check AWS credentials or bucket permissions.'
        }
    }
}
