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
	}
}
