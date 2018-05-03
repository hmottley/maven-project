pipeline {
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to staging') {
            steps {
                build job: 'DeployToStaging'
            }
        }
        stage ('Deploy to Prod') {
            steps {
                timeout(time:5, unit:'DAYS') {
                    input message:'Approve Production Deployment?'
                }
                build job: 'ProdDeploy'
            }
            post {
                success {
                    echo 'Code deployed to Production'
                }
                failure {
                    echo 'Deployment Failed'
                }
            }
        }
    }
}
