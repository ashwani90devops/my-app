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
}
