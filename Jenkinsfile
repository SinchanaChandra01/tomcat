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
                sshagent(['ubuntu-ssh-key']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no sample.war ubuntu@3.110.187.21:/home/ubuntu/tomcat/webapps/
                        scp -o StrictHostKeyChecking=no sample.war ubuntu@43.204.24.249:/opt/tomcat/tomcat10/webapps
                    '''
                }
            }
        }  
        stage('Test') {
            steps {
                // Verify Tomcat servers are responding
                sh '''
                    echo "Checking Tomcat server 1..."
                    curl -f http://3.110.187.21:8080/sample/ || exit 1

                    echo "Checking Tomcat server 2..."
                    curl -f http://43.204.24.249:8080/sample/ || exit 1
                '''
            }
        }
    }
}
