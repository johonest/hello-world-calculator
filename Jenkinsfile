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
            environment {
                TOMCAT_URL = 'http://18.136.203.150:8080/'  // Update with your Tomcat server URL
                CONTEXT_PATH = 'hello'             // Update with your web application's context path
                WAR_FILE = 'target/mywebapp.war'      // Update with your WAR file location relative to project root
            }
            
            steps {
                // Deploy the WAR file to Tomcat
                sh "curl --upload-file ${WAR_FILE} ${TOMCAT_URL}/manager/text/deploy?path=/${CONTEXT_PATH}"
            }
    }
  }
}