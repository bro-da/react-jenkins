pipeline {
    agent any 
     environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
        // DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }
    stages {
        stage('helm pull') { 
            steps {
                sh 'helm repo add bitnami https://charts.bitnami.com/bitnami'
                sh 'helm pull bitnami/postgresql' 
            }
        }
        stage('extract'){
            steps{
                sh 'tar -zxvf postgresql-*'
                sh 'rm -rf postgresql-*.tgz'
            }
        }
        stage("change values in helm-maven"){
            steps{
                //  sh 'sed -i "s/tag: ""/tag: "$USER"/g" mavenhelm/values.yaml'
                sh 'echo "changed values'
            }
        }
        stage("compress file for exporting"){
            steps{
                sh 'tar cvzf postgresql.${BUILD_NUMBER} postgresql-*'
            }
        }
    }
}

