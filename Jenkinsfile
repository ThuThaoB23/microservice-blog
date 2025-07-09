pipeline {
  agent any

  environment {
    SERVICE_DIR = 'user-service'
  }

  stages {
    stage('Clone code') {
      steps {
        git branch: 'main', url: 'https://github.com/ThuThaoB23/microservice-blog.git'
      }
    }

    stage('Install dependencies') {
      steps {
        dir("${SERVICE_DIR}") {
          sh 'npm install'
        }
      }
    }

    stage('Build NestJS app') {
      steps {
        dir("${SERVICE_DIR}") {
          sh 'npm run build'
        }
      }
    }
  }

  post {
    success {
      echo "✅ Đã clone và build thành công NestJS app!"
    }
    failure {
      echo "❌ Build thất bại!"
    }
  }
}
