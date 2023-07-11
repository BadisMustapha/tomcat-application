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
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
		
       }	       	     	         
}
