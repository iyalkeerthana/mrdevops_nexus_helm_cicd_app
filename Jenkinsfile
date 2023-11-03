pipeline {
    agent {
        docker {
            image 'maven'
        }
    }
    stages {
        stage('Test') {
            steps {
                sh 'mvn --version'
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                      sh 'mvn clean package sonar:sonar'
                }
                }
                
            }
        }
    }
}
