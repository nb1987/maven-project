pipeline {
    agent any

    parameters {
        string(
            name: 'tomcat_staging', 
            defaultValue: '3.87.9.82', 
            description: 'Staging Server'
        )
        string(
            name: 'tomcat_prod', 
            defaultValue: '18.212.6.198', 
            description: 'Production Server'
        ) 
    }

    tools {
        maven 'installedMaven'
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                sh 'echo $PATH'
                sh 'mvn clean package'
                sh "sudo docker build . -t tomcatwebapp:${env.BUILD_ID}"
            }
        }

    }
}