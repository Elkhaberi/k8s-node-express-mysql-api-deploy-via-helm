pipeline {
  agent { label 'kaniko'}
  
  stages {

    stage('Build and push to registry') {
      steps {
        container('kaniko') {
          sh '''executor \
                --destination=docker.io/elkhaberorepo/k8s-node-express-mysql-api-deploy-via-helm:$(date -u +%Y-%m-%dT%H%M%S) \
                --context=git://github.com/elkhaberi/k8s-node-express-mysql-api-deploy-via-helm/api/.git#refs/heads/master
          '''
        }
      }
    }

  }
}