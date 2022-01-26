pipeline{
     environment
    {
        MYSQL_SERVER_IP='serverdb.mysql.database.azure.com'
        MYSQL_USERNAME='admin1'
        MYSQL_PASSWORD='Aa22557646'
        registry = "pipeline1233/pipeline" 

        registryCredential = 'pipeline1233' 
        
        dockerImage = ''  
    }
    agent none
    stages {    
          stage( ' permission ' ){
         agent any

            steps {
                sh "sudo chown root:jenkins /run/docker.sock"
            }

        } 
             stage( ' suppression image docker ' ){
                 agent any
                     steps {
                         script {
                           
                            sh 'docker ps -aq | xargs --no-run-if-empty docker stop' 
                echo ' remove all docker containers' 
                sh 'docker ps -aq | xargs --no-run-if-empty docker rm'
                            }
                        }
        } 
            stage( ' Build - Maven package ' ){
                 agent any
                     steps {
                         script {
                               echo ' mvn  clean install '
                             echo 'mvn -Denv.MYSQL_SERVER_IP=${MYSQL_SERVER_IP}  -Denv.MYSQL_USERNAME=${MYSQL_USERNAME} -Denv.MYSQL_PASSWORD=${MYSQL_PASSWORD} package -P MySQL '
                          
                          }
                        }
        }
           
        stage('Docker Build and Tag') { 
             agent any
           steps { 
               
                sh 'sudo docker build -t pipeline .' 
               
                sh 'sudo docker tag samplewebapp pipeline1233/pipeline:latest' 
               
          } 
        }
        
        stage('run image'){
             agent any
             
            steps
   { 
                sh 'sudo  docker run  -d   -p 8003:8080 --name imagepet samplewebapp ' 
 
            } 
        } 
        stage('Publish image to Docker Hub') {
          agent any
            steps {
        withDockerRegistry([ credentialsId: "pipeline1233", url: "" ]) {
          
          sh  'sudo docker push  pipeline1233/pipeline:latest' 
        }
                  
          }
        }
    }
}
