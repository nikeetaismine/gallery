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
        stage("Running the server") {
            steps {
                sh 'node server.js &'
            }
        }
        
    }
}
