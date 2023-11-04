pipeline {
    agent any
    stages {
        stage('SQ Code Build') {
            agent {
                docker {
                    image 'maven:3.9-amazoncorretto-17'
                }
            }
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                    sh 'mvn -e clean package sonar:sonar'
                 }

                }
                
            }
        }
        stage ('Quality Gate Status') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
    }
}

// node {
//   stage('SCM') {
//     checkout scm
//   }
//   stage('SonarQube Analysis') {
//     def mvn = tool 'Default Maven';
//     withSonarQubeEnv() {
//       sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=meha_project_ci -Dsonar.projectName='meha_project_ci'"
//     }
//   }
// }