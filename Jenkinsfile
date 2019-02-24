pipeline {
    agent any 
    stages {
        stage ('Build') {
            steps {
                sh '/Users/bagnalln/Documents/apache-maven-3.6.0/bin/mvn clean package'
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