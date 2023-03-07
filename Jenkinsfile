pipeline {
    agent any
    stages {
        /* "Build" and "Test" stages omitted */

        stage('Installing Dependency, Build Image') {
            steps { 
                sh '''
                uname -a
                npm -v
                docker -v
                npm install
                docker build -t docker.io/adipurnamk/helm-demo:v1.0 .
                '''
            }
        }

        stage('Sanity Check and Vulnerability Testing') {
            steps {
                sh 'snyk container test docker.io/adipurnamk/helm-demo:v1.0'
                // input "Does the staging environment look ok?"
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh """
                docker push adipurnamk/helm-demo:v1.0
                """
            }
        }
    }
}