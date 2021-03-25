pipeline {
    agent any
    stages {
        stage('Build Application'){
            steps {
                bat 'mvn clean package'
            }
        }     
        stage('Sonar Analysis'){
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack  -Dsonar.host.url=http://localhost:9000 -Dsonar.login=d4a63f959878a2d7645dba23b49f7c41804bd579 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java "
                }
            }
        }
        stage('Deploy Application'){
            steps {
                deploy adapters: [tomcat8(credentialsId: 'Login_TomCat', path: '', url: 'http://localhost:8001/')], contextPath: 'caetano-jenkins', war: 'target/caetano-jenkins-0.0.1-SNAPSHOT.war'
            }
        }  
    }
    post {
         sleep(5)
         bat 'echo fim !'
    }
}
