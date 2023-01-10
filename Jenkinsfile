pipeline {
    agent any
       stages {
          stage('Git pull') {
             steps {
                git 'https://github.com/Pritam-Khergade/student-ui.git'
            }
        }
          stage('Build') {
             steps {
                sh 'sudo apt-get update -y'
                sh 'sudo apt install maven -y '
                sh 'mvn clean package'
                sh 'sudo mkdir -p /artifact > /dev/null && sudo chown jenkins: /artifact'
                sh 'sudo mv  /var/lib/jenkins/workspace/${JOB_NAME}/target/studentapp-2.2-SNAPSHOT.war /artifact/student-${BUILD_ID}.war'            }
        }
    }
         stage('docker ') {
            steps {
                sh '''
                sudo apt install docker.io -y
                sudo systemctl start docker && sudo systemctl enable docker
                cp -rv /artifact/student-${BUILD_ID}.war student-${BUILD_ID}.war
                
cat << EOF > dockerfile
FROM tomcat
COPY student-${BUILD_ID}.war ./webapps/
EXPOSE 8080
EOF
                sudo docker build -t tomcatcustomimage .
                '''
            }
        } 
        stage('artifact push to s3 bucket') {
            steps {
                sh 'sudo apt install awscli -y'
                sh 'sudo aws s3 ls'
                sh 'aws s3 cp /artifact/student-${BUILD_ID}.war s3://test-artifact-devops'
            }
        }
    }
}
