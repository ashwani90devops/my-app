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
	
   stage('Sonar Publish'){
	   withCredentials([string(credentialsId: 'sonarqube', variable: 'sonarToken')]) {
        def sonarToken = "sonar.login=${sonarToken}"
        sh "${mvn} sonar:sonar -D${sonarUrl}  -D${sonarToken}"
	 }
      
   }
	
   stage('Mvn Package'){
	   // Build using maven
	   
	   sh "${mvn} clean package"
   }
   
   stage('Email Notification'){
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
