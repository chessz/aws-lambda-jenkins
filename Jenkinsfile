pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        timeout(time: 5, unit: 'MINUTES')
        ansiColor('xterm')
    }
    environment {
        AWS_ID = credentials("AWS_ID")      
        AWS_ACCESS_KEY_ID = "${env.$AWS_ID_USR}"
        AWS_SECRET_ACCESS_KEY = "${env.AWS_ID_PSW}"    
    }
    stages {
        stage('Checkout'){
            steps {
              step([$class: 'WsCleanup'])
              git(
                  url: 'git@github.com:chessz/aws-lambda-jenkins.git',
                  credentialsId: 'jenkins',
                  branch: "add-creds"
                 )
            }
        }
        stage('Build & Deploy lambdas'){
            steps {
               sh "echo Deploying serverless lambdas via Cloudformation.."
                sh "serverless config credentials --provider aws --key ${AWS_ACCESS_KEY_ID}  --secret ${AWS_SECRET_ACCESS_KEY} -o"
               sh "cd py-lambda && serverless deploy"
            }
        }
        stage('TruffleHog Docker Pull') {
            steps {
               sh "docker pull dxa4481/trufflehog"
            }
        }
        stage('TruffleHog Scanning..') {
            steps {
                sh "docker run --rm -v `pwd`:/proj dxa4481/trufflehog --regex --entropy=False https://github.com/cloudyr/aws.secrets"
            }
            post {
                success {
                    echo 'No secrets founds. Well Done!'
                 }
                 failure {
                    mail to: 'mzul@pm.me',
                    subject: "Secrets found in repo: ${currentBuild.fullDisplayName}",
                    body: "Please review this pipeline and repository urgently! : ${env.BUILD_URL}"
                 }
            }
        }
    }
}
