pipeline {
    agent any
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/vignesh755/HTS.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t htsimage /var/lib/jenkins/workspace/kubernetes'
                sh 'sudo docker tag htsimage vignesh755/htsimage:latest'
                sh 'sudo docker tag htsimage vignesh755/htsimage:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push bvignesh755/htsimage:latest'
                sh 'sudo docker image push bvignesh755/htsimage:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/pipeline/pod.yaml'
                sh 'kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}

