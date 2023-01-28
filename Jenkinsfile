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

    stage('Static Analysis') {
      steps {
        sh '''mvn clean -DskipTests=true verify sonar:sonar \\
  -Dsonar.projectKey=Petclinic-Spring \\
  -Dsonar.host.url=http://3.110.233.247:9000 \\
  -Dsonar.login=sqp_a6dda4685e6bd10aa4fc4eeb7b7caa592a0b146e'''
      }
    }

    stage('Unit Test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('QA') {
      parallel {
        stage('Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('Performance Test') {
          steps {
            sh './mvnw verify'
            junit '**/target/surefire-reports/*'
          }
        }

      }
    }

  }
}