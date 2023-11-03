pipeline {
    agent none
    stages {
        stage('Front-End') {
            agent {
                docker {
                    image 'maven'
                }
            }
            steps {
                sh 'mvn --version'
            }
        }
        stage('Java Code Build') {
            agent any
            steps {
                checkout scm
                sh 'mvn clean package'
                
                withSonarQubeEnv(credentialsId: 'sonar-token') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
}

