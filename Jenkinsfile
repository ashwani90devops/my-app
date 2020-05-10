node {
   // This is to demo github action	
   def sonarUrl = 'sonar.host.url=http://192.168.1.2:9000'
   def mvn = tool (name: 'maven-jenkins', type: 'maven') + '/bin/mvn'


   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
	credentialsId: 'ashwani90devops', 
	url: 'https://github.com/ashwani90devops/my-app'
   
   }
	
   stage('Mvn Package'){
	   // Build using maven
	   
	   sh "${mvn} clean package"
   }
   
   stage('Email Notification'){
		mail bcc: '', body: '''Hi Team,

Build deployed successfully.

Thanks,
Ashwani Padhi''', cc: '', from: '', replyTo: '', subject: 'Pipeline Jenkins Job', to: 'ashwani90devops@gmail.com'
   
   }
   
   stage('Slack Notification'){
	slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'jenkins-ashwani-devops', color: 'good', message: 'Welcome to Jenkins, Slack!!!', teamDomain: 'ashwani90devops.slack.com', tokenCredentialId: 'jenkins-slack', username: 'ashwani90devops@gmail.com'
   }
	
}
