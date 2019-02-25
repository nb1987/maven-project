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
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh "scp -i ~/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_staging}:/opt/tomcat/webapps"
                    }
                }

                stage('Deploy to Production') {
                    steps {
                        sh "scp -i ~/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/opt/tomcat/webapps"
                    }
                }
            }
        }
    }
}