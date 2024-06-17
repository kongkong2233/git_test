pipeline {
    agent any

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
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                withAWS(credentials: 'aws_yubin') {
                    sh 'aws s3 cp build/libs/apirdsdemo-0.0.1-SNAPSHOT.jar s3://yubin-build-files/'
                }
            }
        }
    }
}
