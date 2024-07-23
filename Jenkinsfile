pipeline {
    agent any
	environment {
		NVD_API_KEY = credentials('nvdapikey')
	}
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
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
		stage('OWASP Dependency-Check Vulnerabilities') {
            steps {
				echo "Generating Dependency Check"
				dependencyCheck(additionalArguments: '--format XML --format HTML', odcInstallation: 'OWASP Dependency-Check Vulnerabilities')
				echo "Done"
			}
			post {
				always {
					dependencyCheckPublisher(pattern: '**/dependency-check-report.xml')
				}
			}
        }
    }
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}