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
		
		stage('Sonar Amalysis'){
			steps{
				echo "STEP - Run the Sonar Qube analysis on the taret branch"
				withSonarQubeEnv("Test_Sonar")
					{
						bat "mvn sonar:sonar"
						}
				}
		}
		
		
	}
}