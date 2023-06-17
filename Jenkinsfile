pipeline{ 

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
    stage('Manual Approval') {
        steps {
            script {
                input message: 'Apakah anda mau melakukan deploy?', ok: 'Proceed', submitter: 'user1'
            }
        }
    }
    stage ('Deploy') {
      steps {
        script {
          // Deploy aplikasi
          deploy adapters: [tomcat9(credentialsId: 'tomcat_credential', path: '', url: 'http://18.136.203.150:8080/')], contextPath: '/hello', onFailure: false, war: 'target/*.war' 
          echo "Aplikasi sudah ter-deploy! Silahkan akses di: http://18.136.203.150:8080/hello"

          // Menunda eksekusi selama 1 menit
          sleep time: 1, unit: 'MINUTES'

          //Mematikan aplikasi
          ssh -i /var/jenkins_home/tomcat-server-key.pem ubuntu@18.136.203.150 'rm -rf /var/lib/tomcat9/webapps/hello'
          echo "Aplikasi sudah dimatikan!"
      }
    }
  }
}

}
