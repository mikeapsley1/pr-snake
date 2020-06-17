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

        stage('Synk') {
         
            // report on all vulnerabilities
            snyk test --json > report.json
            
            // test for high vulnerabilies
            sh "snyk test --severity-threshold=high"
            

            

            
      }
    
  
    /*
    stage('Pull') {
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
     */

}
