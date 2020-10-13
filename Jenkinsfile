pipeline {
    agent any
    stages {
        stage('repo clean & download Truffle Hog') {
            steps {
               sh "rm -rf security-scanner-pipeline"
               sh "docker pull dxa4481/trufflehog
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
    }
}
