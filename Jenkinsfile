pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        timeout(time: 5, unit: 'MINUTES')
        ansiColor('xterm')
    }
    stages {
        stage('repo clean & download Truffle Hog') {
            steps {
               step([$class: 'WsCleanup'])
               sh "docker pull dxa4481/trufflehog"
            }
        }
        stage('scanning') {
            steps {
                sh "docker run --rm -v `pwd`:/proj dxa4481/trufflehog --regex --entropy=False https://github.com/dxa4481/truffleHog"
            }
        }
    }
    post {
        failure {
        mail to: 'mzul@pm.me',
             subject: "Secrets found in repo: ${currentBuild.fullDisplayName}",
             body: "Please review this pipeline and repository urgently! : ${env.BUILD_URL}"
        }
        always {
            echo 'Housekeeping Duty'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'No secrets founds. Well Done!'
        }
    }
}
