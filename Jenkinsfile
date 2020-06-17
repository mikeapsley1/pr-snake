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
         
         // test for high vulnerabilies
         // sh "snyk test --severity-threshold=high"
            
         sh '''
            snyk test --json | jq '. | (.vulnerabilities[] | select(.CVSSv3 | contains("AV:N")))'
            '''
            
      }
    
  
    /*
    stage('Pull') {
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
     */

}
