pipeline {
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
        NEXUS_IP = "3.109.121.237"
    }
    stages {
        stage('SQ Code Quality check') {
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
                       docker build -t $NEXUS_IP:8083/springapp:${VERSION} .
                       docker login -u admin -p $nexus_creds $NEXUS_IP:8083
                       docker push $NEXUS_IP:8083/springapp:${VERSION}
                       docker rmi $NEXUS_IP:8083/springapp:${VERSION}
                     '''
                   }
                }
            }
        }
        stage ('Arrest the misconfigurations using datree in helm charts') {
            steps {
                script {
                    dir('kubernetes/myapp') {
                        sh 'helm datree config set offline local'
                        sh 'helm datree test .'
                 }
                }
            }
        }
        stage ('push the helm charts to nexus repo') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'nexus-passwd', variable: 'nexus_creds')]) {
                        dir('kubernetes/') {
                            sh '''
                            helmversion=$(helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ')
                            tar -czvf myapp-${helmversion}.tgz myapp/

                            curl -u admin:$nexus_creds http://$NEXUS_IP:8081/repository/helm-repo/ --upload-file myapp-${helmversion}.tgz -v
                            '''
                        }
                    }
                }
            }
        }
        stage ('Deploy the code into kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'kube-context', variable: 'KUBECONFIG')]) {
                     sh 'kubectl config use-context kubernetes --kubeconfig=$KUBECONFIG'
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
