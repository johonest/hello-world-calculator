pipeline {
  agent any
  tools {
    maven 'maven-3.6.3' 
  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test') {
      steps {
        script {
          try {
            sh 'mvn test'
          } finally {
            junit 'target/surefire-reports/*.xml'
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          deploy adapters: [tomcat9(credentialsId: 'tomcat_credential', path: '', url: 'http://18.136.203.150:8080')], contextPath: '', onFailure: false, war: 'webapp/target/*.war' 
        }
      }
    }
  }
}