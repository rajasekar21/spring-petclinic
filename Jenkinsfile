pipeline {
  agent {
    node {
      label 'Execution_Node'
    }

  }
  stages {
    stage('Compile') {
      steps {
        echo 'Jenkins Job Started'
        sh './mvnw clean compile'
      }
    }

  }
}