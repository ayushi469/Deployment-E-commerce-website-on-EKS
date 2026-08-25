pipeline {
    agent{
        label 'ec2-machine-agent'
    }

    environment {
        FRONTENT_IMAGE="angular_image"
        BACKEND_IMAGE="dotnet_image"
        REGION="us-east-1"
        AWS_ACCOUNT_ID="593901684160"
        IMAGE_TAG="${GIT_COMMIT}"
    }

    stages{
        stage("Authentication to ECR") {
            steps {
                sh '''
                aws ecr get-login-password --region ${REGION} | \
                docker login --username AWS --password-stdin \
                ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com
                '''
            }
        }

        stage("Build Frontend Image") {
            steps {
                dir('ui') {
                    sh 'docker build -t ${FRONTENT_IMAGE}:${IMAGE_TAG} .'
                }
            }
        }

        stage('Push Frontend image to ECR') {
            steps {
                sh '''
                docker tag ${FRONTENT_IMAGE}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/frontend-repo:${IMAGE_TAG}
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/frontend-repo:${IMAGE_TAG}
                '''
            }
        }

        stage("Build Backend image") {
            steps {
                dir('api') {
                    sh 'docker build -t ${BACKEND_IMAGE}:${IMAGE_TAG} .'
                }
            }
        }

        stage("Push to ECR Backend repo") {
            steps {
                sh '''
                docker tag ${BACKEND_IMAGE}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/backend-repo
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/backend-repo
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline successfully run."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}