pipeline {

    agent any

    // environment{

    // }

    stages{

        stage('Deploy to AWS'){
            agent{
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args "--entrypoint=''"
                }
            }

          
            steps{
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]){
                    sh '''
                        aws --version
                        aws ecs register-task-definition --cli-input-json file://aws/task-definition.json 
                        aws ecs update-service --cluster LearnJenkinsApp-Cluster-Prod --service LearnJenkinsApp-Service-Prod --task-definition LearnJenkinsApp-TaskDefinition-Prod:2
                    '''
                }
            }
        }

        // stage('AWS'){
        //     agent{
        //         docker {
        //             image 'amazon/aws-cli'
        //             reuseNode true
        //             args "--entrypoint=''"
        //         }
        //     }
        //     environment{
        //         AWS_S3_BUCKET = 'learn-jenkins-bucket-535454d'
        //     }
        //     steps{
        //         withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
        //             // some block
        //             sh '''
        //                 aws --version
        //                 aws s3 sync ./build s3://$AWS_S3_BUCKET
        //             '''
        //         }
                
        //     }
        // }
    }
}