node {
   // Defining variable	
   def sonarUrl = 'sonar.host.url=http://192.168.1.2:9000'
   def mvn = tool (name: 'maven-jenkins', type: 'maven') + '/bin/mvn'


   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
	credentialsId: 'ashwani90devops', 
	url: 'https://github.com/ashwani90devops/my-app'
   
   }
		
   stage('MVN Package'){
	   // Build using maven
	   
	   sh "${mvn} clean package"
   }
	
   stage('SonarQube Analysis'){
           withSonarQubeEnv('sonarqube') { 
             sh "${mvn} sonar:sonar"
		   mvn sonar:sonar \
 			 -Dsonar.projectKey=my-app \
 			 -Dsonar.host.url=http://192.168.1.2:9000 \
		         -Dsonar.login=99d5314cf7bbcc86308f4ce140c2c1ef614c2746
	 }
      
   }	
   	
   
   stage('E-mail Notification'){
		mail bcc: '', body: """Hi Team,

Build deployed successfully.
currentResult : ${currentBuild.currentResult}
JOB_NAME : ${env.JOB_NAME}
BUILD_NUMBER : ${env.BUILD_NUMBER}
BUILD_URL : ${env.BUILD_URL}

Thanks,
Ashwani Padhi""", cc: '', from: '', replyTo: '', subject: 'Pipeline Jenkins Job', to: 'ashwani90devops@gmail.com'
   
   }
   
   stage('Slack Notification'){
	slackSend baseUrl: 'https://hooks.slack.com/services/',
		channel: 'jenkins-ashwani-devops',
		color: 'good',
		message: "*${currentBuild.currentResult}:* ${env.JOB_NAME} #${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
		teamDomain: 'ashwani90devops.slack.com',
		tokenCredentialId: 'jenkins-slack',
		username: 'ashwani90devops@gmail.com'
   }
	
}
