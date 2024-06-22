pipeline{
    agent any
    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        timestamps()
    }
    environment{
        imagename = "gulnaz1357/test"
        version="v1"    
    }
    
    stages{
        stage('Building image') {
            steps{
                script {
                    sh "docker build -t ${imagename}:${version} ."
                }
            }
        }
        stage('Push Image to DockerHub') {
            steps{
                script {
                    withCredentials([string(credentialsId: 'docker-hub-credentials', variable: 'dockerhubpwd')]) {
                    sh '''
                        docker login -u gulnaz1357 -p ${dockerhubpwd}
                        docker push ${imagename}:${version}
                    '''
                }
               }
            }
        }
    }
}
