node{
    
    def mavenHome = tool name: "maven 3.8.6"	
    
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
