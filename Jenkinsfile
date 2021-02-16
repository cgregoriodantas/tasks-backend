pipeline{
    agent any
    environment{
        MVN = tool 'MVN_LOCAL'
        scannerHome = tool 'SONAR_SCANER'
    }
    stages{
        stage('Build Backend'){
            steps{
                sh "${MVN}/bin/mvn clean package -DskipTests=true"
            }
        }
        
        stage('Unit Tests'){
            steps{
                sh "${MVN}/bin/mvn test"
            }
        }

        stage('Sonar Anaysis'){
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=409ede2263b0a9feffe50dd65d0e0572930c993a -Dsonar.java.binaries=target"
                }
            }
        }

        stage('Quality Gate'){
            steps{
                sleep(20)
                timeout(time:1, unit:'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
                
            }
        }
    }    
}

