pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
	stages {
		stage('Checkout Github'){
			steps {
        git branch: 'main', credentialsId: 'dd20150224', url: 'https://github.com/dd20150224/NodeApp.git'
			}
    }
		stage('Install node dependencies'){
			steps {
				sh 'npm install'
			}
		}
		stage('Test Code'){
			steps {
				sh 'npm test'
			}
		}
    stage('Build Docker Image') {
      steps {
        docker.build("nodeimage${BUILD_NUMBER}")
      }
    }
	}

	post {
		success {
			echo 'Build&Deploy completed succesfully!'
		}
		failure {
			echo 'Build&Deploy failed. Check logs.'
		}
	}
}
