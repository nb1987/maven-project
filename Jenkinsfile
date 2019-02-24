pipeline {
    agent any 

    tools {
        maven 'installedMaven'
    }

    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }
        stage ('Deploy to Prod') {
            steps {
                timeout(time: 5, unit: 'DAYS') {
                    input message: 'Approve PROD Deployment?'
                }
                /* in event of approval, deploy-to-prod will run */
                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to production'
                }
                failure {
                    echo 'OH NOES! Deployment FAILED!'
                }
            }
        }
    }
}