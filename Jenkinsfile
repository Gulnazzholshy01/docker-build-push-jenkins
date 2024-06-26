pipeline{
    agent any

    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        timestamps()
    }
 
    environment{
        AWS_ACCOUNT_ID="700930849074"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="demo-ecr"
        IMAGE_TAG="v_${env.BUILD_NUMBER}"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    
    stages{

        //Building the Image
        stage('Building image') {
            steps{
                script {
                    sh "docker build -t $REPOSITORY_URI:$IMAGE_TAG ."
                }
            }
        }

        //Logging into AWS ECR
        stage('Logging into AWS ECR') {
            steps {
                script {
                    sh "aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                    }
                }
            }


        //Pushing image into AWS ECR
        stage('Pushing into ECR') {
            steps{ 
                script {
                    sh "docker push $REPOSITORY_URI:$IMAGE_TAG"
                }
            }
        }
    }

    post {
        success {
            // Triggering app-deploy pipeline 
            build job: 'app-deploy', parameters: [string(name: 'IMAGE_TAG', value: "${IMAGE_TAG}")]
        }
    }
}
