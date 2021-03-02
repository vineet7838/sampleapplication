pipeline {
    agent any 
	parameters {
		string (name: 'Environment', 'defaultValue': 'develop');
		string (name: 'dockerPort', 'defaultValue': '6000');

	}
	options{
	

	timeout( time : 1 ,unit: 'HOURS')
	skipDefaultCheckout()
	buildDiscarder(logRotator(daysToKeepStr: '10',numToKeepStr:'10'))
	disableConcurrentBuilds()
	
	}
	
	stages{
	
		stage('Checkout'){
			steps{
				echo "STEP - Checkout branch"
				checkout scm 
				}
		}
		
		stage('Build'){
			steps{
				echo "STEP - Build the branch"
				bat "mvn clean install"
				}
		}
		
		stage('Test'){
			steps{
				echo "STEP - Test the Program using UitCases"
				bat "mvn test"
				}
		}
		
		stage('Sonar Analysis'){
			steps{
				echo "STEP - Run the Sonar Qube analysis on the taret branch"
				withSonarQubeEnv("Test_Sonar")
					{
						bat "mvn sonar:sonar"
						}
				}
		}
		
		stage('Upload to Artifactory'){
			steps{
				echo "STEP - Upload the artifact to artifactory"
				
				rtMavenDeployer(
				id : 'deployer'
				serverId: '123456789@artifactory',
				releaseRepo: 'CI-Automation-JAVA',
				snapshotRepo: 'CI-Automation-JAVA'
				)
				
				rtMavenRun(
				pom: 'pom.xml',
				goals: ' clean install ',
				deployerId: 'deployer' 				
				
				)
				rtPublishBuildInfo(
				serverId : '123456789@artifactory'
				)
				
				}
		}
		
		stage('Docker Image'){
		steps{
				echo "STEP - Build the docker image"
				bat "docker build -t Build1 --no-cache -f Dockerfile ."
				}
		}
		
		
		
	}
}