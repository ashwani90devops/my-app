node {
   // Defining variable	
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
	
    stage('Deploy Dev'){
	echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"    
	deploy adapters: [tomcat8(credentialsId: 'deployer', path: '', url: 'http://52.87.252.45:8080/')], 
		contextPath: null, 
		onFailure: false, 
		war: "**/*.war"
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
		channel: '#jenkins-ashwani-devops',
		color: 'good',
		message: "*${currentBuild.currentResult}:* ${env.JOB_NAME} #${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
		teamDomain: 'ashwani90devops.slack.com',
		tokenCredentialId: 'jenkins-slack',
		username: 'jenkins'
   }               
	
}
