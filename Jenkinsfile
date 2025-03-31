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
                script {
                    try {
                        sh 'npm test'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
    }
	post {
        success{
            script {
                def buildId = "${env.BUILD_NUMBER}"
                def renderUrl = credentials('RENDER_URL')
                def message = "  *Deployment Successful!* \n\n  
                *Build Number:* ${buildId} \n\n  
                *Project:* ${env.JOB_NAME} \n\n  
                *Live Site:* <${renderUrl}| Click Here to View> \n\n"

                sh "
                    curl -X POST -H 'Content-type: application/json' --data '{
                        \"text\": \"${message}\"
                    }' $SLACK_WEBHOOK_URL
                "
            }
        }
		failure {
			emailext subject: "Jenkins Test Failure!",
			body: "Tests failed for ${env.JOB_NAME} #${env.BUILD_NUMBER}. \nCheck Jenkins logs for details.",
			to: "denise.mwangi@student.moringaschool.com",
			from: "jenkins@galleryexample.com"
		}
	}
}
