pipeline {
  agent {
    node {
      label 'tr-aws-n2'
    }

  }
  stages {
    stage('compile') {
      steps {
        sh './mvnw clean compile'
        echo 'jenkin started'
      }
    }

  }
}