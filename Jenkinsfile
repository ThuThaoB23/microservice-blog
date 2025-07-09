pipeline {
  agent {
    docker {
      image 'thaott084/node-docker-cli:2.0'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  environment {
    NODE_ENV = 'production'
    SERVICE_DIR = 'user-service' // Thay đổi nếu muốn build service khác
  }

  stages {
    stage('Clone source') {
      steps {
        checkout scm
        script {
          dir("${SERVICE_DIR}") {
            sh 'ls -la' // Kiểm tra đã vào đúng folder
          }
        }
      }
    }

    stage('Install dependencies') {
      steps {
        dir("${SERVICE_DIR}") {
          sh 'npm install'
        }
      }
    }

    stage('Build service') {
      steps {
        dir("${SERVICE_DIR}") {
          sh 'npm run build'
        }
      }
    }

    stage('Archive build output (optional)') {
      when {
        expression { fileExists("${SERVICE_DIR}/dist") }
      }
      steps {
        archiveArtifacts artifacts: "${SERVICE_DIR}/dist/**/*", fingerprint: true
      }
    }
  }

  post {
    success {
      echo "✅ Build ${SERVICE_DIR} completed"
    }
    failure {
      echo "❌ Build ${SERVICE_DIR} failed"
    }
  }
}
