pipeline {
    agent none
    stages {
        stage('Back-end') {
            agent {
                docker {
                    image 'node:16-alphine'
                }
            }
            steps {
                sh 'node --version'

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
