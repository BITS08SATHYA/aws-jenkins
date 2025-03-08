pipeline {

    agent any

    // environment{

    // }

    stages{

        stage('AWS'){
            agent{
                docker {
                    image 'amazon/aws-cli'
                    args "--entrypoint=''"
                }
            }
            steps{
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    // some block
                    sh '''
                        aws --version
                        echo "Hello S3!" > index.html
                        aws s3 cp index.html s3://learn-jenkins-bucket-535454d/index.html
                    '''
                }
                
            }
        }
    }
}