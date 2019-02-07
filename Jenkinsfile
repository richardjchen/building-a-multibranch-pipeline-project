properties = null
properties1 = null

def loadProperties(envfile) {
  node {
         dir('envDir') {
             git url: 'https://github.com/richardjchen/building-a-multibranch-pipeline-project.git'
    	 }
	  
	  echo "${envDir}"
	  
	  def exists = fileExists 'envDir/jenkins.properties'
	  if (exists){
    		echo "jenkins.properties exists"
	  } else {
	       echo "jenkins.properties does not exist"
	  }
	  
	 properties = readProperties file: envfile
	 properties1 = readProperties file: 'jenkins.properties'
         echo "Immediate one ${properties1.ACR_LOGINSERVER}"
  }
}

pipeline {
    agent any
    environment {
        CI = 'true'
        branch = 'master'
	scmUrl = 'https://github.com/richardjchen/building-a-multibranch-pipeline-project.git'
	development = './config-manager/dev/jenkins.properties'
        production = './config-manager/prod/jenkins.properties'
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
                     echo "build114 branch successful!"
	             loadProperties(development)
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
	      	      echo "build114 branch successful!"
		      loadProperties(production)
	              echo "Running build on git repo ${properties.ACR_LOGINSERVER} branch ${properties.ACR_NAMESPACE}"
	         }
              }
          }
      }
}
