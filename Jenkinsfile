pipeline {
    agent any
    stages {
        stage ('Workspace clean') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage ('check user permissions') {
            agent {
                docker {
                    image 'maven'
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
                sh 'whoami'
                sh 'chown -R root:root $HOME/.m2'
            }
        }    
        // stage ('SonarQube Code Check') {
        //     agent {
        //         docker {
        //             image 'maven'
        //             args '-v $HOME/.m2:/root/.m2'
        //         }
        //     }
        //     steps {
        //         script {
        //             sh 'chown -R root:root $HOME/.m2'
        //             withSonarQubeEnv(credentialsId: 'sonar-token') {
        //                 sh 'mvn -e clean package sonar:sonar'
        //             }

        //         }
               
        //     }
        // }
    }
}

