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
                sh '''
                    aws --version
                '''
            }
        }
    }



}