pipeline {
    agent any
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
		steps {
            echo "Generating Dependency Check"
            dependencyCheck(additionalArguments: '--format XML --format HTML', odcInstallation: 'OWASP-Dependency-Check')
            echo "Done"
        }
        post {
            always {
                dependencyCheckPublisher(pattern: '**/dependency-check-report.xml')
            }
	}
    }
}