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
           withSonarQubeEnv('sonar') { 
             sh "${mvn} sonar:sonar"
	 }
      
   }	
    stage("Quality Gate Statuc Check"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                   slackSend baseUrl: 'https://hooks.slack.com/services/',
                   channel: '#jenkins-ashwani-devops',
                   color: 'danger', 
                   message: "*${currentBuild.currentResult}:* ${env.JOB_NAME} #${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}", 
                   teamDomain: 'ashwani90devops.slack.com',
                   tokenCredentialId: 'jenkins-slack',
	           username: 'jenkins'
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }    	
	
    stage('deploy-dev'){
       def tomcatDevIp = '192.68.1.24'
	   def tomcatHome = '/opt/tomcat8/'
	   def webApps = tomcatHome+'webapps/'
	   def tomcatStart = "${tomcatHome}bin/startup.sh"
	   def tomcatStop = "${tomcatHome}bin/shutdown.sh"
	   
	   sshagent (credentials: ['tomcat-dev']) {
	      sh "scp -o StrictHostKeyChecking=no target/myweb*.war root@${tomcatDevIp}:${webApps}myweb.war"
          sh "ssh ec2-user@${tomcatDevIp} ${tomcatStop}"
		  sh "ssh ec2-user@${tomcatDevIp} ${tomcatStart}"
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
		channel: '#jenkins-ashwani-devops',
		color: 'good',
		message: "*${currentBuild.currentResult}:* ${env.JOB_NAME} #${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
		teamDomain: 'ashwani90devops.slack.com',
		tokenCredentialId: 'jenkins-slack',
		username: 'jenkins'
   }               
	
}
