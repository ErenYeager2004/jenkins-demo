pipeline {
	agent any
	stages {
		stage('Checkout') {
			steps {
				checkout scm
			}
		}
	
		stage('Build') {
			steps {
				sh 'mvn clean package'
			}
		}

		stage('Docker Build') {
			steps {
				sh 'docker build -t jenkins-demo:latest .'
			}
		}
		
		stage('Deploy') {
			steps {
				sh '''
					docker stop jenkins-demo || true
					docker rm jenkins-demo || true
					docker run -d --name jenkins-demo -p 8081:8080 jenkins-demo:latest
				'''
			}
		}
	}
}
