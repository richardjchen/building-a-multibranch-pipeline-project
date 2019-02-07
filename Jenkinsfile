properties = null

def getProperties(envfile, name) {
  node {
         checkout scm	  
	 def exists = fileExists envfile
	 
	 if (exists){
    	       echo "jenkins.properties exists"
    	       properties = readProperties file: envfile
    	       return properties.getProperty(name)
    	       
	 } else {
	       echo "jenkins.properties does not exist"
	       properties = readProperties file: envfile
    	       return properties.getProperty(name)
	 }
    }
}

pipeline {
    agent any
    environment {
        CI = 'true'
        branch = 'master'
	scmUrl = 'https://github.com/richardjchen/building-a-multibranch-pipeline-project.git'
	development = 'config-manager/dev/jenkins.properties'
        production = 'config-manager/prod/jenkins.properties'
    }
    stages {
    	  stage('checkout git') {
              steps {
                  git branch: branch, url: scmUrl
             }
          }	
          stage('Build') {
              steps {
                  echo "build branch successful!"
              }
          }
          stage('Test') {
              steps {
                  echo "test successful!"
              }
          }
          stage('Deliver for development') {
              when {
                  branch 'development' 
              }
              steps {
                 script {
                     echo "build development branch"
	             println getProperties(development, "ACR_LOGINSERVER")
		     echo "Running build on git repo ${properties.ACR_LOGINSERVER} branch ${properties.ACR_NAMESPACE}"
       		  }
              }
          }
          stage('Deploy for production') {
              when {
                  branch 'production'  
              }
              steps {
	          script {
	      	     echo "build production branch"
	             println getProperties(production, "ACR_LOGINSERVER")
	             echo "Running build on git repo ${properties.ACR_LOGINSERVER} branch ${properties.ACR_NAMESPACE}"
	         }
              }
          }
      }
}
