pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        timeout(time: 5, unit: 'MINUTES')
        ansiColor('xterm')
    }
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }
    stages {
        stage('Checkout'){
            steps {
              git(
                  url: 'git@github.com:chessz/aws-lambda-jenkins.git',
                  credentialsId: 'jenkins',
                  branch: "${branch}"
    )
            }
        }
        stage('TruffleHog Docker Pull') {
            steps {
               step([$class: 'WsCleanup'])
               sh "docker pull dxa4481/trufflehog"
            }
        }
        stage('TruffleHog Scanning..') {
            steps {
                sh "docker run --rm -v `pwd`:/proj dxa4481/trufflehog --regex --entropy=False https://github.com/dxa4481/truffleHog"
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
