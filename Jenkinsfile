pipeline {
    agent any 
    stages {
        stage ('Build') {
            steps {
                sh 'export PATH=$PATH:/Users/bagnalln/Documents/apache-maven-3.6.0/bin'
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}