String payload = "${payload}"
def jsonObject = readJSON text: payload
String gitHash = "${jsonObject.pull_request.head.sha}"
String buildUrl = "${BUILD_URL}"
String gitStatusPostUrl = "https://4c1db18fade840749b3cccb3519beaa198602495:x-oauth-basic@api.github.com/repos/mikeapsley1/pr-snake/statuses/${gitHash}"
    

node ('ubuntu-slave'){  
    def app
    stage('Cloning Git') {
       
    checkout scm
        
    
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
        
    sh """
    curl -X POST -H "application/json" -d '{"state":"success", "target_url":"${buildUrl}", "description":"Build Success", "context":"build/job"}' "${gitStatusPostUrl}"
       """
        
 }
    
}
