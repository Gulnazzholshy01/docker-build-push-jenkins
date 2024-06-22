pipeline{
    agent any
    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        timestamps()
    }
    environment{
        imagename = "gulnaz1357/test"
        version="v1"
        registry = "<dockerhub-username>/<repo-name>"
        registryCredential = '<dockerhub-credential-name>'        
    }
    
    stages{
       stage('Building image') {
        steps{
            script {
              sh "docker build -t ${imagename}:${version} ."
            }
        }
    }
    //    stage('Deploy Image') {
    //    steps{
    //      script {
    //         docker.withRegistry( '', registryCredential ) {
    //         dockerImage.push()
    //       }
    //     }
    //   }
    // }
    }
}