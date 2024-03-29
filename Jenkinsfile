pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        timeout(time: 5, unit: 'MINUTES')
        ansiColor('xterm')
    }
    environment {
        AWS_ID = credentials("AWS_ID")      
        AWS_ACCESS_KEY_ID = "${AWS_ID_USR}"
        AWS_SECRET_ACCESS_KEY = "${AWS_ID_PSW}"    
    }
    parameters { 
        string(defaultValue: "cloudyr/aws.secrets", description: 'Example public repo', name: 'GITHUB_PROJ')
    }
    stages {
        stage('Checkout'){
            steps {
              step([$class: 'WsCleanup'])
              git(
                  url: 'git@github.com:chessz/aws-lambda-jenkins',
                  credentialsId: 'jenkins',
                  branch: "add-creds"
                 )
            }
        }
        stage('Compile & Unit Test'){   
            steps {
               sh "echo Compiling application..."
               sh "cd py-lambda && python -m compileall ."
            }
        }
        stage('TruffleHog Docker Pull') {
            steps {
               sh "docker pull dxa4481/trufflehog"
            }
        }
        stage('TruffleHog Scanning..') {
            steps {
               // this is a private repo hence no auth supported hence using static for now
               sh "docker run --rm -v `pwd`:/proj dxa4481/trufflehog --regex --entropy=False https://github.com/${params.GITHUB_PROJ}"
               //sh "docker run --rm -v `pwd`:/proj dxa4481/trufflehog --regex --entropy=False https://github.com/cloudyr/aws.secrets"
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
        stage('Build & Deploy lambdas'){
            steps {
               sh "echo Deploying serverless lambdas via Cloudformation.."
               sh "serverless config credentials --provider aws --key ${AWS_ACCESS_KEY_ID}  --secret ${AWS_SECRET_ACCESS_KEY} -o"
               sh "cd py-lambda && serverless deploy"
            }
        }
    }
}
