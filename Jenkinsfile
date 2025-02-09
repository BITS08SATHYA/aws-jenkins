pipeline {

    agent any

    environment{

    }

    stages{

        stage('AWS'){
            agent{
                docker {
                    image 'amazon/aws-cli'
                }
            }
            steps{
                sh '''
                    aws --version
                '''
            }
        }

        stage('Build'){
            agent {

            }
        }

    }



}