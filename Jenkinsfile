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
		   steps {
			   withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
				   sh 'mvn snyk:test -fn'
			   }
		   }
	   }
	   stage('Build') {
		 steps {
			 withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
				 script{
				 app = docker.build("asg")
				 }
			 }
		 }
		 }
		  stage('Push') {
		 steps {
			  script{
			         docker.withRegistry('https://582269253552.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
				 app.push("latest")
				 }
			  }
		 }
	}
  }
}
