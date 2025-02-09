pipeline {

    agent any

    // environment{

    // }

    stages{

        stage('AWS'){
            agent{
                docker {
                    image 'amazon/aws-cli'
                    args '-u root'
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