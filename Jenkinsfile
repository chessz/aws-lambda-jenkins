pipeline {
    agent any
    stages {
        stage('repo clean & download Truffle Hog') {
            steps {
               sh "rm -rf security-scanner-pipeline"
               sh "git clone https://github.com/dxa4481/truffleHog.git"
               sh "echo Downloaded!!!"
            }
        }
        stage('install') {
            steps {
                sh "echo 'lets run install'"
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
