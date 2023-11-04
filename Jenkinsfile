pipeline {
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
    }
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
        stage ('Docker build & Push image to Nexus') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'nexus-passwd', variable: 'nexus_creds')]) {
                     sh '''
                       docker build -t 13.126.92.181:8083/springapp:${VERSION} .
                       docker login -u admin -p $nexus_creds 13.126.92.181:8083
                       docker push 13.126.92.181:8083/springapp:${VERSION}
                       docker rmi 13.126.92.181:8083/springapp:${VERSION}
                     '''
                   }
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