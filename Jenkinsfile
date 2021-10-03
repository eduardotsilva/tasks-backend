pipeline {
  agent any
  stages {
    stage('Build Backend') {
      steps {
        bat 'mvn clean package -DskipTests=true'
      }
    }

    stage('Unit Tests') {
      steps {
        bat 'mvn test'
      }
    }

    stage('Sonar Analysis') {
      environment {
        scannerSonarHome = tool 'SONAR_SCANNER'
      }
      steps {
        withSonarQubeEnv('SONAR_LOCAL') {
          bat  "${scannerSonarHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=42c5b056e6465138926cbed4394dadc10bbe1142 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Aplication.java"
        }
      }
    }

    stage("Quality Gate") {
        steps {
            sleep(5)
            timeout(time:2, unit: 'MINUTES'){
                waitForQualityGate abortPipeline: false
            }
        }
     }

    stage("Deploy Backend"){
        steps{
            deploy adapters: [tomcat8(credentialsId: 'tomcat_login', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
        }
    }

    stage("API Test"){
        steps{
            dir('api-test'){
              git credentialsId: 'github_login', url: 'https://github.com/eduardotsilva/tasks-api-test'
              bat 'mvn test'
            }
        }
    }

     stage("Deploy Frontend"){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/eduardotsilva/tasks-frontend'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'tomcat_login', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
      }

    stage("Functional Test"){
            steps{
                dir('functional-test'){
                  git credentialsId: 'github_login', url: 'https://github.com/eduardotsilva/tasks-functional-test'
                  bat 'mvn test'
                }
            }
        }

  }
}



