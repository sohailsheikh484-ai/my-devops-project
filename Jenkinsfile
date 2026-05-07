pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sohailsheikh/html-app:latest"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                     url: 'https://github.com/sohailsheikh484-ai/my-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
        export KUBECONFIG=/var/lib/jenkins/.kube/config
        kubectl get nodes
        kubectl apply -f deployment.yaml
        kubectl apply -f service.yaml
        kubectl rollout restart deployment html-app
        '''
            }
        }
    }
}
