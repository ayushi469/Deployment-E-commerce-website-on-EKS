pipeline {
    agent{
        label 'ec2-machine-agent'
    }

    environment {
        FRONTENT_IMAGE="angular_image:latest"
        BACKEND_IMAGE="dotnet_image:latest"
        REGION="us-east-1"
        AWS_ACCOUNT_ID="593901684160"
    }

    stages{
        stage("Authentication to ECR") {
            steps {
                sh 
                '''
                aws ecr-get-login-password --region ${REGION} | \
                docker login --username AWS --password-stdin \
                ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com
                '''
            }
        }

        stage("Build Frontend Image") {
            step {
                dir('ui') {
                    sh 'docker build -t ${FRONTENT_IMAGE}:${GIT_COMMIT_ID} .'
                }
            }
        }

        stage('Push Frontend image to ECR') {
            steps {
                sh 
                '''
                docker tag ${FRONTENT_IMAGE}:${GIT_COMMIT_ID} ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/frontend-repo
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/frontend-repo
                '''
            }
        }

        stage("Build Backend image") {
            steps {
                dir('api') {
                    sh 'docker build -t ${BACKEND_IMAGE}:${GIT_COMMIT_ID} .'
                }
            }
        }

        stage("Push to ECR Backend repo") {
            steps {
                sh 
                '''
                docker tag ${BACKEND_IMAGE}:${GIT_COMMIT_ID} ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/backend-repo
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/backend-repo
                '''
            }
        }
    }
}