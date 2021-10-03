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
          bat "${scannerSonarHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=3ec3e224af99ca57b9e62d4a4836e5d2ec550368 -Dsonar.java.binaries=target "
        }
      }
    }

  }
}



