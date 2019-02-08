@Library('shared-jenkins-lib')

pipeline {
    agent any
    environment {
        CI = 'true'
        branch = 'master'
	scmUrl = 'https://github.com/richardjchen/building-a-multibranch-pipeline-project.git'
	development = './config-manager/dev/jenkins.properties'
        production = './config-manager/prod/jenkins.properties'
	serverName = "QUNSX0xPR0lOU0VSVkVS"
    }
    stages {
    	  stage('checkout git') {
              steps {
                  git branch: branch, credentialsId: 'richard.jqchen@gmail.com', url: scmUrl
             }
          }	
          stage('Build') {
	      steps {
		  echo "build branch successful"
              }
          }
          stage('Test') {
              steps {
                  echo "test successful"
              }
          }
	  stage('check workspace files') {
              steps {
                  
		  script {
		    	 def  FILES_LIST = sh (script: "ls   '${workspace}'", returnStdout: true).trim()
	                 //DEBUG
                         echo "FILES_LIST : ${FILES_LIST}"
                         //PARSING
                         for(String ele : FILES_LIST.split("\\r?\\n")){ 
                             println ">>>${ele}<<<"     
                         }	  
       		  }
              }
          }     
          stage('Deliver for development') {
              when {
                  branch 'development' 
              }
              steps {
                 script {
		     echo "build114 branch successful!"
		     println getProperties(development, serverName)
       		  }
              }
          }
          stage('Deploy for production') {
              when {
                  branch 'production'  
              }
              steps {
	          script {
	               echo "build114 branch successful!"
		       println getProperties(production, serverName)
	         }
              }
          }
      }
}
