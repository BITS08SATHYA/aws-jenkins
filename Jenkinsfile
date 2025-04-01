pipeline {

    agent any

    environment{
        AWS_DEFAULT_REGION = 'us-east-1' // Default AWS region for CLI commands
        AWS_ECS_CLUSTER = 'LearnJenkinsApp-Cluster-Prod'
        AWS_ECS_SERVICE_PROD = 'LearnJenkinsApp-Service-Prod'
        AWS_ECS_TASK_DEF = 'LearnJenkinsApp-TaskDefinition-Prod'
    }

    stages{

        stage('Build'){
            agent{
                docker {
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

        stage('Build Docker image'){
            agent{
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args "-u root -v /var/run/docker.sock:/var/run/docker.dock --entrypoint=''"
                }
            }
            steps{
                sh '''
                    amazon-linux-extras install docker 
                    docker build -t myjenkinsApp .
                '''
            }
        }

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
                        aws ecs update-service --cluster $AWS_ECS_CLUSTER --service $AWS_ECS_SERVICE_PROD --task-definition $AWS_ECS_TASK_DEF:$LATEST_TD_REVISION
                    '''
                }
            }
        }
// aws ecs wait services-stable --cluster $AWS_ECS_CLUSTER --service $AWS_ECS_SERVICE_PROD
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