node{
    
    def mavenHome = tool name: "maven 3.8.6"	
    
	try{
	slacknotification("STARTED")
	stage('CheckoutCode'){
		git branch: 'development', credentialsId: 'cabe19fe-9cfb-4164-b03d-48bc19111d51', url: 'https://github.com/VinodChenga1211/maven-web-application.git'
	}
	
	stage('Build'){
		sh "${mavenHome}/bin/mvn clean package"	
	}
	
	stage('ExecuteSonarQubeReport'){
	    sh "${mavenHome}/bin/mvn sonar:sonar"
	}
	
	stage('UploadArtifactsintoNexus'){
	    sh "${mavenHome}/bin/mvn deploy"
	}
	
	stage('DeployAppintoTomcatServer'){
	    sshagent(['91dd7e00-412c-4e09-b2d9-2f1f7a11f91e']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.39.178:/opt/apache-tomcat-9.0.69/webapps/"
        } 
	}
	}
	catch(e){
		currentBuild.result ="FAILURE"
		throw e
	}finally{
		slacknotification(currentBuild.result)
	}
		
} //node closing

//slack notification code
def slacknotification(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'ORANGE'
  def colorCode = '#FF8C00'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'ORANGE'
    colorCode = '#FF8C00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#2cae6b'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
