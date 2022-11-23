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
                            sh "docker network create localhost"
                            sh "docker run --rm -it -d --network=localhost --name=${env.service_name} ${env.service_name}"
                            sh "docker run --rm -t -d --network=localhost owasp/zap2docker-stable zap-full-scan.py -t http://${env.service_name}:3000"
                            sh "docker stop ${env.service_name}"
                     }
               }
         }
   }

}
