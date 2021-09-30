pipeline {
  agent any 
  tools {
    maven 'maven'
  }
   parameters {
    string defaultValue: '', description: '', name: 'INPUT_LOCATION', trim: true
    }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    
     stage('Analysis') {
    steps {
      script {
        dir(INPUT_LOCATION) {
          files = findFiles(glob: '*.*')
        }       
        files.each { f ->
          def TASK_COLLECTION = [:]
          TASK_COLLECTION["MOBSF"] =  {
 			def AUTH_KEY = '1e524fb43bf44dfe33dceddd05b32b2be9ca5f8f3abd786e184ba8663e1c3374'
    			upload_cmd = "curl -s -S -F 'file=@${env.INPUT_LOCATION}/${f}' http://localhost:8002/api/v1/upload -H 'Authorization:${AUTH_KEY}'"
       			upload_result = sh label: 'Upload Binary', returnStdout: true, script: upload_cmd
                      
    			def response_map = readJSON text: upload_result
    			def app_type = response_map["scan_type"]
       			def app_hash = response_map["hash"]
       			def app_name = response_map["file_name"]
                      
    		scan_start_cmd = "curl -s -S -X POST --url http://localhost:8002/api/v1/scan --data 'scan_type=${app_type}&file_name=${app_name}&hash=${app_hash}' -H 			'Authorization:${AUTH_KEY}'"
       		sh label: 'Start Scan of Binary', returnStdout: true, script: scan_start_cmd

          }
          TASK_COLLECTION["Qark"] = {
		sh "qark --apk ${env.INPUT_LOCATION}/${f}"
          }
          parallel(TASK_COLLECTION) 
        }
      }
    }
  } 
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
  /*    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    }
    
  stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.232.202.25:/prod/apache-tomcat-8.5.39/webapps/webapp.war'
              }      
           }       
    }
    
    
    stage ('DAST') {
      steps {
        sshagent(['zap']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@13.232.158.44 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://13.232.202.25:8080/webapp/" || true'
        }
      }
    }*/
    
  }
}
