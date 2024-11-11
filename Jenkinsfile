pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
  environment {
    DOCKER_HUB_CREDENTIALS_ID = 'dockerhub-jenkins-token'
    // DOCKER_REGISTRY = 'https://hub.docker.com/u/dd20150224'
    DOCKER_HUB_REPO = 'dd20150224/nodeapp'
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
        script {
          dockerImage = docker.build("${DOCKER_HUB_REPO}:latest")
        }
      }
    }
    stage('Push Image to DockerHub') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIALS_ID}") {
            dockerImage.push('latest')
          }
        }
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
