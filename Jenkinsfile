pipeline {

    agent any

    environment{
        AWS_DEFAULT_REGION = 'us-east-1' // Default AWS region for CLI commands
    }

    stages{

        stage('Deploy to AWS'){
            agent{
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args "-u root --entrypoint=''"
                }
            }

          
            steps{
                withCredentials([usernamePassword(credentialsId: 'my-aws', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]){
                    sh '''
                        aws --version
                        yum install jq -y
                        LATEST_TD_REVISION=$(aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json | jq '.taskDefinition.revision')
                        echo $LATEST_TD_REVISION
                        aws ecs update-service --cluster LearnJenkinsApp-Cluster-Prod --service LearnJenkinsApp-Service-Prod --task-definition LearnJenkinsApp-TaskDefinition-Prod:$LATEST_TD_REVISION
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