pipeline {
  agent any 
     
        
  tools {
    maven 'maven'
  }
        
  environment {
    Snyk = 'Snyk'
    Trivy = 'Trivy'
    Audit = 'Audit'
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
    
     
 stage ('Check-Git-Secrets') {
      steps {
        sh 'sudo chmod 666 /var/run/docker.sock'
        //sh 'sudo docker run raphaelareya/gitleaks:latest -r https://github.com/securitis/CICD.git'       
        //sh 'rm trufflehog || true'
        //sh 'docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
        //sh 'cat trufflehog'
      }
    }  
         
   stage ('Container Scan') {
       steps {
              sh 'echo Container Scan'
            //sh 'docker run aquasec/trivy:0.18.3 vulnerables/phpldapadmin-remote-dump'
            //sh 'docker run aquasec/trivy:0.18.3 repo https://github.com/securitis/CICD.git'
            //sh 'docker run aquasec/trivy:0.18.3 image vulnerables/web-dvwa:latest'
             }
             } 
   
  stage ('Snyk') {
     steps {
             snykSecurity failOnError: false, failOnIssues: false, organisation: 'securitis', projectName: 'CICDSelf', severity: 'high', snykInstallation: 'snyk', snykTokenId: 'snykid', targetFile: '/var/lib/jenkins/workspace/SelfCICD'
             //  sh 'snykSecurity failOnError: false, failOnIssues: false, organisation: 'self', projectName: 'CICDSelf', severity: 'high', snykTokenId: 'snykid', targetFile: '/var/lib/jenkins/workspace/SelfCICD''     
            //sh 'snykSecurity failOnError: false, failOnIssues: false, organisation: 'self', projectName: 'CICDSelf', severity: 'high', snykInstallation: 'snyk', snykTokenId: 'snykid', targetFile: '/var/lib/jenkins/workspace/SelfCICD''
            //sh 'sudo cp -r /var/lib/jenkins/workspace/DevDemo /var/www/html' 
            //sh 'chmod +777 /var/lib/jenkins/workspace/CICD/target/WebApp'
            //sh 'ls /var/www/html'
            //sh 'sudo cp -r /var/lib/jenkins/OWASP-Dependency-Check/reports /var/www/html'
       }
    }
      
   
   
   stage ('Build') {
      steps {
      sh 'mvn clean package'
     }
    }
 

    stage ('Deploy') {
     steps {
            sh 'chmod +777 /var/lib/jenkins/workspace/DevDemo'
            sh 'sudo cp -r /var/lib/jenkins/workspace/DevDemo /var/www/html' 
            //sh 'chmod +777 /var/lib/jenkins/workspace/CICD/target/WebApp'
            //sh 'ls /var/www/html'
            //sh 'sudo cp -r /var/lib/jenkins/OWASP-Dependency-Check/reports /var/www/html'
       }
    }
     
    
    
 
    
  }
} 



