pipeline {
    agent {
        docker {
            image 'node:lts-bullseye-slim' 
            args '-p 3000:3000' 
        }
        environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
        }

    }
    agent {
        environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
        }
    }
    stages {

        stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/bro-da/react-jenkins.git'])
 
      }
    }
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    }
}

