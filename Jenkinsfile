pipeline {
   agent any
   stages {
       stage('Code Quality') {
           steps {
               echo 'Sonar Analysis Started'
               sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://54.186.236.36:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_46d0a95f27e94895e5824ac9a042781a4a096a3f" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
               echo 'Sonar Analysis Completed'
           }
       }
      
       stage('Build LMS') {
           steps {
               echo 'LMS Build Started'
               sh 'cd webapp && npm install && npm run build'
               echo 'LMS Build Completed'
           }
       }
      
       stage('Publish LMS') {
           steps {
            
                   sh "zip webapp/lms.zip -r webapp/dist"
                   sh "curl -v -u admin:monty333 --upload-file webapp/lms.zip http://54.186.236.36:8081/repository/lms/"
               }
           }
       
      
       stage('Deploy LMS') {
           steps {
            
                   sh "curl -u admin:monty333 -X GET \'http://54.186.236.36:8081/repository/lms/lms.zip' --output lms.zip"
                   sh 'sudo rm -rf /var/www/html/*'
                   sh "sudo unzip -o lms.zip"
                   sh "sudo cp -r webapp/dist/* /var/www/html"
               }
           }
       stage('Clean Up Workspace') {
           steps {
                   echo 'Cleaning Work Space'
                   // Install Cleanup Workspace plugin to make below command work
                   cleanWs()
           }
       }
   }
}
