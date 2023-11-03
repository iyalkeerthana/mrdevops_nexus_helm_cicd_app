pipeline {
    agent {
        docker {
            image 'maven'
            args '-v $HOME/.m2:/root/.m2'
        }
    }
    stages {
        stage('SonarQube Code Check') {
            steps {
                script {
                    sh 'whoami' // Check the user running the pipeline inside the Docker container
                    sh 'chown -R root:root $HOME/.m2' // Apply permissions using root user
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
    }
}
