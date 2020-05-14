node {
   // Defining variable	
   def sonarUrl = 'sonar.host.url=http://192.168.1.2:9000'
   def mvn = tool (name: 'maven-jenkins', type: 'maven') + '/bin/mvn'
   def tomcatIp = '192.168.1.24'
   def tomcatUser = 'root'
   def stopTomcat = "ssh ${tomcatUser}@${tomcatIp} /opt/tomcat8/apache-tomcat-8.5.24/bin/shutdown.sh"
   def startTomcat = "ssh ${tomcatUser}@${tomcatIp} /opt/tomcat8/apache-tomcat-8.5.24/bin/startup.sh"
   def copyWar = "scp -o StrictHostKeyChecking=no target/myweb.war ${tomcatUser}@${tomcatIp}:/opt/tomcat8/apache-tomcat-8.5.24/webapps/"

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
	
    stage('Deploy Dev'){
	   sh 'mv target/myweb*.war target/myweb.war' 
	   
       sshagent(['tomcat-dev']) {
			sh "${stopTomcat}"
			sh "${copyWar}"
			sh "${startTomcat}"
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
