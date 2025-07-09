pipeline {
  agent {
    docker {
      image 'thaott084/node-docker-cli:1.0'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  stages {
    stage('Check versions') {
      steps {
        sh 'node -v'
        sh 'docker --version'
      }
    }
  }
}
