pipeline {
    agent any
    stages {
        /* "Build" and "Test" stages omitted */

        stage('Installing Dependency, Build Image') {
            steps { 
                sh '''
                echo "Building version ${BUILD_NUMBER}"
                npm -v
                docker -v
                npm install
                docker build -t docker.io/adipurnamk/helm-demo:v1.${BUILD_NUMBER} .
                '''
            }
        }

        stage('Vulnerability Testing and Sanity Check') {
            steps {
                sh 'snyk container test docker.io/adipurnamk/helm-demo:v1.${BUILD_NUMBER}'
                input "Does the image look ok?"
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh """
                docker push adipurnamk/helm-demo:v1.${BUILD_NUMBER}
                """
            }
        }
    }
}
