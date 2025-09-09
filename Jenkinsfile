pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'wget https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/sample.war -O sample.war'
            }
        }

        stage('Deploy') {
    steps {
        sshagent(['ubuntu-user']) {
            sh '''
              scp -o StrictHostKeyChecking=no sample.war ubuntu@13.233.45.247:/home/ubuntu/
              ssh -o StrictHostKeyChecking=no ubuntu@13.233.45.247 "sudo mv /home/ubuntu/sample.war /opt/tomcat/tomcat10/webapps/ && sudo systemctl restart tomcat"
            '''
        }
    }
}

        stage('Test') {
            steps {
                sh 'curl -I http://13.233.45.247:8080/sample/'
            }
        }
    }
}
