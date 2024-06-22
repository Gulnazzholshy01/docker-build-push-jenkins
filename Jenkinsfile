pipeline{
    agent any
    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        timestamps()
    }
    environment{
        AWS_ACCOUNT_ID="700930849074"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="test"
        IMAGE_TAG="v2"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    
    stages{
        
        //Building the Image
        stage('Building image') {
            steps{
                script {
                    sh "docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG ."
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
                    sh "docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $REPOSITORY_URI:$IMAGE_TAG"
                    sh "docker push $REPOSITORY_URI:$IMAGE_TAG"
                }
            }
        }

        //Pushing Docker Images into AWS ECR
        // stage('Push Image to DockerHub') {
        //     steps{
        //         script {
        //             withCredentials([string(credentialsId: 'docker-hub-credentials', variable: 'dockerhubpwd')]) {
        //             sh '''
        //                 docker login -u gulnaz1357 -p ${dockerhubpwd}
        //                 docker push ${imagename}:${version}
        //             '''
        //         }
        //        }
        //     }
        // }
    }
}
