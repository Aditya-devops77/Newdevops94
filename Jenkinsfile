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
            }
        }
    }
}
