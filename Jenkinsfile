pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
	stage('OWASP Dependency Check') {
		steps {
			dependencyCheck additionalArguments: '--format HTML --formal XML --disableYarnAudit', odcInstallation: 'NameAnythingYouWant'
		}
	}
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}

}