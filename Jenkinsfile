pipeline {
    agent none
    stages {
        stage('Back-end') {
            agent {
                docker {
                    image 'nginx'
                }
            }
            steps {
                sh 'nginx --version'

            }
        }
        stage('front-end') {
            agent {
                docker {
                    image 'maven'
                }
            }
            steps {
                sh 'mvn --version'
            }
        }
    }
}
