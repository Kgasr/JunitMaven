node {
	
	
	stage('Checkout') {
	              git 'https://github.com/Kgasr/JunitMaven.git'
	       }
	
	stage('SonarQube Analysis') {
	
	              withSonarQubeEnv('My SonarQube Server') {
	              bat "C:/sonar-scanner-3.0.3.778-windows/bin/sonar-scanner"
	              }
	       }
	
	 stage('UnitTesting') {
	     dir('abc'){
	       try  {
	                     bat 'mvn test'
	       }
	         
	     
	        catch(exc) {
	       stage('JiraBug'){
	       withEnv(['JIRA_SITE=LOCAL']){
	              def testIssue = [fields: [ project: [key: 'RJ'],
	                           summary: 'New JIRA Created from Jenkins.',
	                           description: 'New JIRA Created from Jenkins.',
	                          issuetype: [name: 'Bug']]]
	
	                     response = jiraNewIssue issue: testIssue
	                     echo response.successful.toString()
	                     echo response.data.toString()
	                    }    
	              }
	         error('Unit Testing Failed so stopping pipeline')
	       }
	     }
	       stage('BuildCode'){
	           dir('abc'){
	        bat 'mvn clean install'
	           }
	  }
	  } 
	}

