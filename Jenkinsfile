node {
   // This is to demo github action	
   def sonarUrl = 'sonar.host.url=http://192.168.1.2:9000'
   def mvn = tool (name: 'jenkins-maven', type: 'maven') + '/bin/mvn'


   stage('SCM Checkout'){
    // Clone repo
	git branch: 'master', 
	credentialsId: 'ashwani90devops', 
	url: 'https://github.com/ashwani90devops/my-app'
   
   }
	
   stage('Mvn Package'){
	   // Build using maven
	   
	   sh "${mvn} clean package deploy"
   }
   
   stage('Email Notification'){
		mail bcc: '', body: '''Hi Team,

        Build deployed successfully.

        Thanks,
        Ashwani Kumar Padhi''', cc: '', from: '', replyTo: '', subject: 'Pipeline Jenkins Job', to: 'ashwani90devop@gmail.com'
   
   }
}
