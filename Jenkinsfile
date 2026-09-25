pipeline {
	agent none
	stages {
		stage('Checkout') {
			agent {
				label 'Build-Agent'
			}
			steps {
				checkout scm
			}
		}
	
		stage('Build') {
			agent {
				label 'Build-Agent'
			}
			steps {
				sh 'mvn clean package'
				stash name: 'app-jar', includes: 'target/*.jar'
			}
		}

		stage('Docker Build') {
			agent{
				label 'Build-Agent-1'
			}
			steps {
				unstash 'app-jar'
				sh 'docker build -t jenkins-demo:latest .'
				sh 'docker save -o jenkins-demo.tar jenkins-demo:latest'
				stash name: 'docker-image',
					includes: 'jenkins-demo.tar'
			}
		}
		
		stage('Deploy') {
			agent {
					label 'Build-Agent-2'
			}
			steps {
				unstash 'docker-image'
				sh '''
					docker load -i jenkins-demo.tar
					docker stop jenkins-demo || true
					docker rm jenkins-demo || true
					docker run -d --name jenkins-demo -p 8081:8080 jenkins-demo:latest
				'''
			}
		}
	}
}
