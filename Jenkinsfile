pipeline {

    agent any

    // environment{

    // }

    stages{

        stage('Build'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('AWS'){
            agent{
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args "--entrypoint=''"
                }
            }
            environment{
                AWS_S3_BUCKET = 'learn-jenkins-bucket-535454d'
            }
            steps{
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    // some block
                    sh '''
                        aws --version
                        aws s3 sync build s3://$AWS_S3_BUCKET
                    '''
                }
                
            }
        }
    }
}