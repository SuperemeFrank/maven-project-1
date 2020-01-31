pipeline {
    agent any
	parameters{
		string(name: 'tomcat_dev', defaultValue: '3.12.104.197', description: 'Staging Server')
		string(name: 'tomcat_prod', defaultValue: '18.222.60.250', description: 'Production Server')
	}

	tools{
		maven 'local maven'
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages {
		stage ('Build'){
			steps {
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'start archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		stage ('Deployments') {
			parallel{
				stage ('Deploy to Staging'){
					steps{
						sh "scp -i /Users/haoyu/FrankWorkSpace/AWS/tomcat-demo.pem **/target/*.war ec2-user${tomcat_dev}:/var/lib/tomcat8/webapps"
					}
				}
				stage ('Deploy to Production'){
					steps{
						sh "scp -i /Users/haoyu/FrankWorkSpace/AWS/tomcat-demo.pem **/target/*.war ec2-user${tomcat_prod}:/var/lib/tomcat8/webapps"
					}
				}
			}
		}
	}
}
