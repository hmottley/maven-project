pipeline {
    agent any
	parameters {
	    string(name: 'tomcat_dev', defaultValue: 'staging', description: 'staging server')
	    string(name: 'tomcat_prod', defaultValue: 'prod', description: 'prod server')
	}
	triggers{
	    pollSCM('* * * * *')
	}
    stages{
        stage('Build'){
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
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp **/target/*.war /opt/tomcat/apache-tomcat-9.0.7-${params.tomcat_staging}/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war /opt/tomcat/apache-tomcat-9.0.7-${params.tomcat_prod}/webapps/"
                    }
                }
            }
        }
    }
}
