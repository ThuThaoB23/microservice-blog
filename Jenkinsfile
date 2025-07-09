pipeline {
  agent {
    label 'node-agent'
  }

  stages {
    stage('Build') {
      steps {
        sh 'node -v'
        sh 'npm install'
        sh 'npm run build'
      }
    }
  }
}