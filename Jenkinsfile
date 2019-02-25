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

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh "scp -i /Users/bagnalln/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
                    }
                }

                stage('Deploy to Production') {
                    steps {
                        sh "scp -i /Users/bagnalln/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}