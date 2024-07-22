pipeline {
    agent any
    stages {
		stage('Install Node.js and npm') {
            steps {
                sh '''
                curl -sL https://deb.nodesource.com/setup_14.x | bash -
                sudo apt-get install -y nodejs
                '''
            }
        }
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
    }
}