pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      label 'kaniko'
      yaml """
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug-539ddefcae3fd6b411a95982a830d987f4214251
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /root
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: regcred
          items:
            - key: .dockerconfigjson
              path: .docker/config.json
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
            /kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --skip-tls-verify --cache=true --destination=docker.io/elkhaberirepo/node-app:kaniko
            '''
        }
      }
    }
  }
}
