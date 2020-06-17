node ('ubuntu-slave'){  
    def app
    stage('Cloning Git') {
       checkout scm
    }  
    
 stage('Build') {
       app = docker.build("mikebroomfield/snake")
   }
    
    stage('Push') {
     docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
            app.push("latest")
        			}
         }
    
     stage('Trivy Scan') {
         // sh "trivy --no-progress --exit-code 0 python:3.4-alpine"
         sh "trivy --no-progress --exit-code 1 --severity HIGH,CRITICAL -f json -o results.json python:3.4-alpine"
      }
  
    
    stage('Pull') {
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
     

}
