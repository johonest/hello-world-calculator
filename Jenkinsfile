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
          input message: 'Apakah Anda mau melakukan deploy?', ok: 'Proceed', submitter: 'user1'
        }
      }
    }
    stage ('Deploy') {
      steps {
        script {
          // Deploy aplikasi langsung ke tomcat di EC2
          //deploy adapters: [tomcat9(credentialsId: 'tomcat_credential', path: '', url: 'http://18.136.203.150:8080/')], contextPath: '/hello', onFailure: false, war: 'target/*.war' 
          //echo "Aplikasi sudah ter-deploy! Silahkan akses di: http://18.136.203.150:8080/hello"

          // Menunda eksekusi selama 1 menit
          //echo "Aplikasi akan berjalan selama 1 minutes..."
          //sleep time: 1, unit: 'MINUTES'

          // Mematikan aplikasi
          //echo "Mematikan aplikasi..."
          //sh "ssh ubuntu@18.136.203.150 'sudo rm -r /var/lib/tomcat9/webapps/hello && sudo rm /var/lib/tomcat9/webapps/hello.*'"
          //echo "Aplikasi sudah dimatikan. Pipeline sukses!"

          //Deploy aplikasi ke Docker
          docker.image('tomcat:latest').withRun { c ->
                        // Copy the WAR file to the container
                        docker.cp('target/hello.war', "${c.id}:/usr/local/tomcat/webapps/ROOT.war")

                        // Restart the container to deploy the WAR file
                        docker.restart(c.id)
        }
      }
    }
  }
}
