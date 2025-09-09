pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Downloading calendar.war..."
                sh 'wget -O calendar.war https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war'
            }
        }

        stage('Deploy') {
            steps {
                sshagent([''ubuntu-ssh-key']) {
                    sh '''
                    echo "Deploying to Tomcat1 (43.204.37.216)..."
                    scp -o StrictHostKeyChecking=no calendar.war ubuntu@43.204.37.216:/tmp/
                    ssh ubuntu@98.84.151.139 "sudo mv /tmp/calendar.war /opt/tomcat10/webapps/ && sudo systemctl restart tomcat"

                    echo "Deploying to Tomcat2 (13.233.89.141)..."
                    scp -o StrictHostKeyChecking=no calendar.war ubuntu@13.233.89.141:/tmp/
                    ssh ubuntu@34.228.19.124 "sudo mv /tmp/calendar.war /opt/tomcat10/webapps/ && sudo systemctl restart tomcat"
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                echo "Testing deployments..."
                sh 'curl -I http://43.204.37.216:8080/calendar/'
                sh 'curl -I http://13.233.89.141:8080/calendar/'
            }
        }
    }
}
