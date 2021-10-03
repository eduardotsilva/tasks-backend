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
        exclusaoCobertura = "sonar.coverage.exclusions= '**/.mvn/**, **/src/test/**,**/model/**,**Aplication.java'"
      }
      steps {
        withSonarQubeEnv('SONAR_LOCAL') {
          bat "${scannerSonarHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=42c5b056e6465138926cbed4394dadc10bbe1142 -Dsonar.java.binaries=target sonar.coverage.exclusions= '**/.mvn/**, **/src/test/**,**/model/**,**Aplication.java'"
        }
      }
    }

  }
}



