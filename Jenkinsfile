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
                sh 'npm install'
            }
        }
        stage("Running the server") {
            steps {
                sh 'node server.js &'
            }
        }
        stage("Deploying to Heroku") {
            steps {
                withCredentials([usernamecolonpassword(credentialsId: 'heroku', variable: 'HEROKU_AUTH')]) {
                    sh 'git push https://${HEROKU_AUTH}@git.heroku.com/nikeetaismine.git master'
                }
            }
        }
    }
}
