pipeline {
  environment {
    service_name = "${env.GIT_URL.replaceFirst(/^.*\/([^\/]+?).git$/, '$1')}"
  }
  
  agent any
  
  stages {
         stage('Build and Push Docker Image') {
               steps{
                     script {
                            sh "docker build -t ${env.service_name} ."
                     }
               }
         }
   }

}
