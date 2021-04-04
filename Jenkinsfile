pipeline {
  agent {
    label 'kaniko'
  }
  stages {
    stage('Checkout') {
      steps {
        git(url: 'https://github.com/Elkhaberi/k8s-node-express-mysql-api-deploy-via-helm.git', changelog: true, branch: 'master', credentialsId: 'Github', poll: true)
      }
    }

  }
}