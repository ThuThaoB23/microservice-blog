pipeline {
  agent {
    label 'node'
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