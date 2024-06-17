pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-northeast-2'
        S3_BUCKET = 'yubin-build-files1'
        JAR_FILE = 'build/libs/apirdsdemo-0.0.1-SNAPSHOT.jar'
        APP_NAME = 'apirdsdemo'
        DEPLOY_GROUP = 'apirdsdemo-group-name'
        DEPLOY_ZIP = 'deployment.zip'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kongkong2233/git_test.git', credentialsId: 'yubin_github_id'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'chmod 755 ./gradlew'
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // test steps
            }
        }
        stage('Prepare Deployment Package') {
            steps {
                echo 'Preparing deployment package...'
                sh """
                cd deployment
                zip -r ${env.DEPLOY_ZIP} appspec.yml scripts/
                mv ${env.DEPLOY_ZIP} ../
                cd ..
                zip -r ${env.DEPLOY_ZIP} -g build/libs/apirdsdemo-0.0.1-SNAPSHOT.jar
                """
            }
        }
        stage('Upload to S3') {
            steps {
                withAWS(credentials: 'aws_yubin') {
                    s3Upload(bucket: env.S3_BUCKET, file: env.DEPLOY_ZIP)
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                withAWS(credentials: 'aws_yubin') {
                    sh 'aws s3 cp build/libs/apirdsdemo-0.0.1-SNAPSHOT.jar s3://yubin-build-files1/'
                }
            }
        }
    }
}
