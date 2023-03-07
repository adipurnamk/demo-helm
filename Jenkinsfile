pipeline {
    agent any
    stages {
        /* "Build" and "Test" stages omitted */

        stage('Installing Dependency, Build and Test using Snyk') {
            steps { 
                sh '''
                uname -a
                // sudo apt-get update
                // sudo apt-get install npm docker -y
                curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
                nvm install node
                npm -v
                docker -v
                npm install
                docker build -t docker.io/adipurnamk/helm-demo:v1.0 .
                snyk container test docker.io/adipurnamk/helm-demo:v1.0
                '''
            }
        }

        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push adipurnamk/helm-demo:v1.0'
            }
        }
    }
}