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
                sh 'echo "changed values"'
            }
        }
        stage("compress file for exporting"){
            steps{
                sh 'tar cvzf postgresql.${BUILD_NUMBER}.tgz postgresql '
            }
        }
        stage("aws login "){
            steps{
                 sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/l2m3f3d0'
                sh 'aws ecr get-login-password  --region us-east-1 | helm registry login --username AWS --password-stdin 976846671615.dkr.ecr.us-east-1.amazonaws.com'
                sh 'helm push postgresql.${BUILD_NUMBER}.tgz oci://976846671615.dkr.ecr.us-east-1.amazonaws.com/postgresql-images'
            }
        }
    }
}

