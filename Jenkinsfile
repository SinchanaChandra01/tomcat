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
                        scp -o StrictHostKeyChecking=no sample.war ubuntu@15.206.68.84:/home/ubuntu/tomcat/webapps/
                        scp -o StrictHostKeyChecking=no sample.war ubuntu@13.233.140.144:/opt/tomcat/tomcat10/webapps
                    '''
                }
            }
        }  
        stage('Test') {
            steps {
                // Verify Tomcat servers are responding
                sh '''
                    echo "Checking Tomcat server 1..."
                    curl -f http://15.206.68.84:8080/sample/ || exit 1

                    echo "Checking Tomcat server 2..."
                    curl -f http://13.233.89.141:8080/sample/ || exit 1
                '''
            }
        }
    }
}

