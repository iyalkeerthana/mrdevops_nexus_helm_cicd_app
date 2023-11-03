pipeline {
    agent any
    stages {
        stage('SonarQube Code Qaulity Check') {
            agent {
                docker {
                    image 'maven'
                    args '-v /home/ubuntu/sonar:/root/.m2/repository'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') { 
                     sh 'mvn clean package sonar:sonar'
                 }

                }
            }
        }
    }
}