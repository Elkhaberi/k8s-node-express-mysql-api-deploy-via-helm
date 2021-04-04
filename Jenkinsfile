pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      label 'kaniko'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    args: ["--context=git://github.com/Elkhaberi/k8s-node-express-mysql-api-deploy-via-helm","--destination=elkhaberi93/node-express-api:kaniko"]
    volumeMounts:
      - name: kaniko-secret
        mountPath: /kaniko/.docker
  volumes:
  - name: kaniko-secret
    secret:
      secretName: regcred
      items:
      - key: .dockerconfigjson
        path: config.json
"""
    }
  }
  stages {
    stage('Build with Kaniko') {
      environment {
        PATH = "/busybox:/kaniko:$PATH"
      }
      steps {
        git(url: 'https://github.com/Elkhaberi/k8s-node-express-mysql-api-deploy-via-helm.git', 
            changelog: true, 
            branch: 'master', 
            credentialsId: 'Github', 
            poll: true)
        container(name: 'kaniko', shell: '/busybox/sh') {
            sh '''#!/busybox/sh
            /kaniko/executor -f `pwd`/Dockerfile
            '''
        }
      }
    }
  }
}
