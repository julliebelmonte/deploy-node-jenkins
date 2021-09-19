pipeline {
  agent any

  tools { nodejs "node" }

  environment {
    DOCKERHUB_CRED = credentials('dockerhub-token')
  }

  stages { 
    stage('Build') {
      steps {
        sh '''
          docker build -t deploy-node-jenkins:latest .
          docker tag deploy-node-jenkins julliebelmonte/deploy-node-jenkins:latest
          docker tag deploy-node-jenkins julliebelmonte/deploy-node-jenkins:$BUILD_NUMBER
        '''
      }
    }

    stage ('Test') {
      steps {
        sh '''
          npm install
          npm test
        '''
      }
    }

    stage('Publish') {
      steps {
        sh 'echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin'
        sh '''
          docker push julliebelmonte/deploy-node-jenkins:latest
          docker push julliebelmonte/deploy-node-jenkins:$BUILD_NUMBER
        '''
      }
    }
  }

  post {
    always {
      sh 'docker logout'
    }
  }
}