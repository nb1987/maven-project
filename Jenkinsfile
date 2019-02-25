pipeline {
    agent any

    parameters {
        string(
            name: 'tomcat_staging', 
            defaultValue: '174.129.181.153', 
            description: 'Staging Server'
        )
        string(
            name: 'tomcat_prod', 
            defaultValue: '3.88.85.130', 
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
                        sh "scp -i ~/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
                    }
                }

                stage('Deploy to Production') {
                    steps {
                        sh "scp -i ~/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}