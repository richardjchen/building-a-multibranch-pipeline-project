import biz.source_code.base64Coder.Base64Coder

@Grab(group='biz.source_code', module='base64coder')

properties = null

def textEncodeBase64UTF(strEncode) {

    def encodedValue = Base64Coder.encodeString(strEncode)

    println "encodedValue--> ${encodedValue}"
    
    return encodedValueUTF
    
}

def textDecodeBase64UTF(strDecode) {

    def decodedValueUTF = Base64Coder.decodeString(strDecode)
    
    println "decodedValue--> ${decodedValueUTF}"
    
    return decodedValueUTF
}

def getProperties(envfile, name) {
  
	 def keyValue = " "
	 def exists = fileExists envfile
	
	 def encodedName = textEncodeBase64UTF(name)
	 def decodedName = textDecodeBase64UTF(encodedName)
		 
	 if (exists){
    	       echo "jenkins.properties file exists"
    	       properties = readProperties file: envfile

	       if (properties.size() > 0){
    		    echo "jenkins.properties value exists"
		    keys= properties.keySet()
		    keyValue = properties[name]
		    
		    echo "set them up as sytem environment variables for application to grab the value"
		       
		    keys= properties.keySet()
                    for(key in keys) {
                        value = properties["${key}"]
			env."${key}" = "${value}"
                        println "populate them as sytem environment variables: ${value}"
                    }
		       
	        } else {
	             echo "jenkins.properties does not exist"
	       }     	    
    	       
	 } else {
	       echo "jenkins.properties does not exist"
	 }
	
	 echo "jenkins.properties keyValue: ${keyValue}"
	
	 def encodedValue = textEncodeBase64UTF(keyValue)
         return encodedValue
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
	               echo "build114 branch successful!"
		       println getProperties(production, "ACR_LOGINSERVER")
	      	       echo "Running build on git repo ${properties.ACR_LOGINSERVER} branch ${properties.ACR_NAMESPACE}"
	         }
              }
          }
      }
}
