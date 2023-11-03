pipeline {
    agent {
        docker {
            image 'maven'
        }
    }
    stages {
        stage('Test') {
            steps {
                script {
                    sh 'maven --version'
                }
            }
        }
    }
}
