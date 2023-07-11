pipeline{

      agent any
      tools {
           maven 'maven'
           jdk 'java-17'
       }
        
      stages{

        stage('Clone Repo'){
		 steps{
                      script{
		    	                 git branch: 'main', url: 'https://github.com/BadisMustapha/tomcat-application.git'
                      	  }
               	     }  
           
        }
          
        stage('Maven Build'){
		 steps{
                      script{
		    	                bat "mvn clean package"
                      	  }
               	     }  
            
        }

	stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('SonarQub-Server') {
                bat 'mvn sonar:sonar'
              }
            }
          }
	      
          stage("Upload Build Artifact") {
            steps {
              nexusArtifactUploader(
	        nexusVersion: 'nexus3',
	        protocol: 'http',
	        nexusUrl: 'localhost:8081',
	        groupId: 'example.demo',
	        version: '1.0-SNAPSHOT',
	        repository: 'demo-snapshot-repository',
	        credentialsId: 'Nexus',
	        artifacts: [
	            [artifactId: 'helloworld',
	             classifier: '',
	             file: 'target\\helloworld.war',
	             type: 'war']
	        ]
	     )
            }
          }

       stage("Deploy To Tomcat") {
	    steps {
	      deploy adapters: [tomcat9(url: 'http://localhost:9090/', 
                              credentialsId: 'tomcat')], 
                     war: 'target/*.war',
                     contextPath: ''
	    }
  	}
		
       }	       	     	         
}
