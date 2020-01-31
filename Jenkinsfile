pipeline {
    agent any
	tools {
		maven 'local maven'
	}
    stages{
        stage ('build'){
            steps{
				sh 'mvn clean package'
            }
			post{
				success {
					echo 'start archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
					}
				}
			}
		stage ('Deploy to staging...'){
			steps{
				build job: 'deploy-to-staging'
			}
		}
		stage ('Deploy to Production'){
			steps{
				timeout(time:5, unit:'DAYS'){
					input message:'Is deployed to production?'
				}

				build job: 'deploy-to-production'
			}
			post {
				success {
					echo 'successfully deploy it to production!'
				}
				failure {
					echo 'deploy failure'
				}
			}
		}
    }
}
