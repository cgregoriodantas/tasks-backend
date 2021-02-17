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
                sleep(5)
                timeout(time:1, unit:'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
                
            }
        }

        stage('Deploy Backend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'UsuarioTomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        } 

         stage('API Test'){
            steps{
                dir('api-test'){
                    git credentialsId: '5c51777b-c043-47ad-b9f5-587999fcb5da', url: 'https://github.com/cgregoriodantas/tasks-api-test.git'
                    sh "${MVN}/bin/mvn test"                 
                }
            }
        }   

        stage('Deploy Frontend'){
            steps{
                dir('frontend'){
                    git credentialsId: '5c51777b-c043-47ad-b9f5-587999fcb5da', url: 'https://github.com/cgregoriodantas/tasks-frontend.git'
                    sh "${MVN}/bin/mvn clean package"
                    deploy adapters: [tomcat8(credentialsId: 'UsuarioTomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        } 

        /*
        stage('Deploy Prod'){
            steps{
                sh 'docker-compose build'
                sh 'docker-compose up -d'
            }
        }
        */
    }    
}

