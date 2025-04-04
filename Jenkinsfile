pipeline {
    agent any
        environment {
        K8S= credentials('kube-trial')  // Use the stored K8s token
    }
    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/KPkm25/sample-ci-cd.git', branch: 'main'
                echo "Repository cloned successfully."
            }
        } // <-- Missing closing brace added here

        stage('Build') {
            steps {
                sh 'docker build -t sample-ci-cd:latest .'
            }
        }

        stage('Push') {
            steps {
                withDockerRegistry([credentialsId: 'docker-creds', url: '']) {
                    sh 'docker tag sample-ci-cd:latest kpkm25/sample-ci-cd:latest'
                    sh 'docker push kpkm25/sample-ci-cd:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([file(credentialsId: 'kube-trial', variable: 'KUBECONFIG_FILE')]) {
  sh '''
    export KUBECONFIG=$KUBECONFIG_FILE
    kubectl apply -f k8s/deployment.yaml --namespace=jenkins
  '''
}

            }
        }
    }
}
