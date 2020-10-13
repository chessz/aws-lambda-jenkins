pipeline {
    agent any
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
        stage('test') {
            steps {
                sh "echo 'start testing'"
            }
        }
        stage('package') {
            steps {
                sh "echo package it!!!"
            }
        }
    post {
        cleanup {
            /* clean up our workspace */
            deleteDir()
            /* clean up tmp directory */
            dir("${workspace}@tmp") {
                deleteDir()
            }
            /* clean up script directory */
            dir("${workspace}@script") {
                deleteDir()
            }
        }
    }
    }
}
