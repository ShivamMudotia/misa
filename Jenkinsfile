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
                            sh "docker network create localhost 2>/dev/null || true"
                            sh "docker run --rm -it -d --network=localhost --name=${env.service_name} ${env.service_name}"
                            sh "sleep 30"
                            sh "docker run --rm -t -v $PWD:/zap/wrk/:rw --network=localhost owasp/zap2docker-stable zap-full-scan.py -t http://${env.service_name}:3000 -r zap-full-scan-report.html"
                            sh "docker stop ${env.service_name} 2>/dev/null || true"
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: 'zap-full-scan-report.html', reportName: 'Zap_Report', reportTitles: '', useWrapperFileDirectly: true])
                            sh "docker run --rm -v $PWD:/src  ghcr.io/zricethezav/gitleaks:latest  detect --source=/src/ -r /src/gitleaks.json --no-git -v"
                     }
               }
         }
   }



}
