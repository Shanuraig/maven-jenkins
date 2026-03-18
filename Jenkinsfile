pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    tools {
        maven 'Maven3'   // We'll define "Maven3" in Jenkins Global Tool Configuration
    }

    triggers {
        // Pick ONE:
        // 1) For webhooks (recommended): configure GitHub webhook -> no cron here
        // 2) For easy demo: uncomment polling to check every minute
        // pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn -B clean test package'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' // show test results in Jenkins
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success { echo 'Build succeeded 🎉' }
        failure { echo 'Build failed ❌ — check Console Output' }
    }
}