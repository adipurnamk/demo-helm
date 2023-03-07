pipeline {
    agent any
    stages {
        /* "Build" and "Test" stages omitted */

        stage('Installing Dependency, Build Image') {
            steps { 
                sh '''
                echo Building version ${BUILD_NUMBER}
                uname -a
                npm -v
                docker -v
                npm install
                docker build -t docker.io/adipurnamk/helm-demo:v1.${BUILD_NUMBER} .
                '''
            }
        }

        stage('Sanity Check and Vulnerability Testing') {
            steps {
                sh 'snyk container test docker.io/adipurnamk/helm-demo:v1.${BUILD_NUMBER}'
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