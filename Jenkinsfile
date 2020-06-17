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

        stage('Dockle') {
         sh "dockle mikebroomfield/snake:latest"
         //sh "dockle -f json -o results.json --exit-code 1 --exit-level fatal mikebroomfield/snake:latest"
      }
    
  
    /*
    stage('Pull') {
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
     */

}
