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

    stage('static analysis') {
      steps {
        sh '''mvn clean verify -DskipTests=true sonar:sonar \\
  -Dsonar.projectKey=tr-sonar-pet \\
  -Dsonar.host.url=http://43.205.108.233:9000 \\
  -Dsonar.login=sqp_ea7979adc57662613d9889aeafe2a91c468e5bfa'''
      }
    }

    stage('Unit Test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('QA') {
      parallel {
        stage('deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
          }
        }

        stage('performance test') {
          steps {
            sh './mvnw verify'
            junit '**/target/surefire-reports/TEST-*.xml'
            perfReport '**/target/jmeter/results/*'
          }
        }

      }
    }

  }
}