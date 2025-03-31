pipeline {
    agent any
    
    tools {
        nodejs('18.19.1')
    }

    stages {
        stage('Cloning Repository...') {
            steps {
                git url: "https://github.com/nikeetaismine/gallery.git", branch: "master"
            }
        }
        stage('Installing dependencies') {
            steps {
                sh 'rm -rf package-lock.json node_modules'
                sh 'npm install'

            }
        }
        stage("Deploying/running the project") {
            steps {
                sh 'node server.js &'
            }
        }
        stage('Running tests') {
            steps {
                sh 'npm test'
            }
        }
    }
	post {
		failure {
			emailext subject: "Jenkins Test Failure!",
			body: "Tests failed for ${env.JOB_NAME} #${env.BUILD_NUMBER}. \nCheck Jenkins logs for details.",
			to: "denise.mwangi@student.moringaschool.com",
			from: "jenkins@galleryexample.com"
		}
	}
}
