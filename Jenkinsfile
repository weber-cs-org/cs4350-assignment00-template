#!groovy

pipeline {
    agent any
    
    environment {
        MY_ENV = 'true'
    }
    
    stages {
        stage('prepare') {
            steps {
                echo 'Initializing ${env.BUILD_ID} on ${env.JENKINS_URL}'
                sh 'printenv'
                sh 'php --version'
                sh 'composer --version'
            }
        }
        stage('build') {
            steps {
                echo 'Building ${env.BUILD_ID} on ${env.JENKINS_URL}'
                sh 'composer install'
            }
        }
        stage('test') {
            steps {
                echo 'Testing ${env.BUILD_ID} on ${env.JENKINS_URL}'
                sh 'composer grade'
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning ${env.BUILD_ID}'
            sh 'rm -fr vendor'
        }
        success {
            echo 'Success'
        }
        failure {
            echo 'Failure'
            mail to: 'donstringham@weber.edu',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayNamek}",
                body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}
