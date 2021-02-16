pipeline{
    agent any
    environment{
        MVN = '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/MVN_LOCAL/bin/mvn'
    }
    stages{
        stage('Build Backend'){
            steps{
                sh '$MVN clean package -DskipTests=true'
            }
        }
    }
    stages{
        stage('Unit Tests'){
            steps{
                sh '$MVN test'
            }
        }
    }
}