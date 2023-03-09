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
                // input "Does the image look ok?"
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh """
                docker push adipurnamk/helm-demo:v1.${BUILD_NUMBER}
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'sa', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh """
                    gcloud auth activate-service-account --key-file $GOOGLE_APPLICATION_CREDENTIALS
                    gcloud config set project adipurnas-projects
                    gcloud container clusters get-credentials demo-app --zone=asia-southeast1-a
                    sed -i "s/image: myimage:TAG/image: adipurnamk/helm-demo:v1.${BUILD_NUMBER}/" deployment.yaml
                    kubectl version
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                    kubectl get svc
                    """
                }
            }
        }
    }
    post{
        always{
            mail to: "${email}",
            subject: "Jenkins Notification",
            body: "${BUILD_URL}  :: ${BUILD_STATUS}"
        }
    }
}
