node ('ubuntu-slave'){  
    def app
    stage('Cloning Git') {
      // checkout scm
        
    String payload = "${payload}"
    def jsonObject = readJSON text: payload
    String gitHash = "${jsonObject.pull_request.head.sha}"
    
    sh "git checkout ${gitHash}"
        
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
         
         // Build Report
         sh "trivy --no-progress --exit-code 0 -f json -o results.json mikebroomfield/snake"
         
         // Print Report 
         sh "trivy --no-progress --exit-code 0 --severity HIGH,CRITICAL mikebroomfield/snake"
         
         // Fail on high & critical vulnerabilities
        // sh "trivy --no-progress --exit-code 1 --severity HIGH,CRITICAL mikebroomfield/snake"
      }
  

//    stage('Pull') {
//        sh "docker-compose down"
//       sh "docker-compose up -d"	
//    }

    stage('Report Back to Github') {
        
    // TODO
        
 }
    
}
