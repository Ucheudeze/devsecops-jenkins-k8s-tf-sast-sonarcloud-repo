pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp1 -Dsonar.organization=asgbuggywebapp1 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=60c25699b0f37b543c7f286258a5f8578ad3fdf0'
			}
        } 
	   stage('RunSCAAnalysisUsingSnyk') {
		   Steps {
			   withCredentials([string(credentialsId 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
				   sh 'mvn snyk:test -fn'
			   }
		   }
	   }
  }
}
