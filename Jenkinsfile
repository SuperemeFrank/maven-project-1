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
    }
}
