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
                       docker build -t 65.2.6.183:8083/springapp:${VERSION} .
                       docker login -u admin -p $nexus_creds 65.2.6.183:8083
                       docker push 65.2.6.183:8083/springapp:${VERSION}
                       docker rmi 65.2.6.183:8083/springapp:${VERSION}
                     '''
                   }
                }
            }
        }

    }
    post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "dkeerthanabe@gmail.com";  
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