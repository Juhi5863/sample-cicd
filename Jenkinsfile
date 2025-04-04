pipeline {
    agent any
        environment {
        K8S= credentials('kube-trial')  // Use the stored K8s token
    }
    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/Juhi5863/sample-cicd.git', branch: 'main'
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
                withDockerRegistry([credentialsId: '6a280542-7619-4c87-9db0-a1208d2b1bc5', url: '']) {
                    sh 'docker tag sample-ci-cd:latest juhichoudhary/sample-ci-cd:latest'
                    sh 'docker push juhichoudhary/sample-ci-cd:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([file(credentialsId: 'kube-trial', variable: 'KUBECONFIG_FILE')]) {
  sh '''
    export KUBECONFIG=$KUBECONFIG_FILE
    kubectl apply -f juhichoudhary/deployment.yaml --namespace=jenkins
  '''
}

            }
        }
    }
}
