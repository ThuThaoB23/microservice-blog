pipeline {
  agent any

  environment {
    IMAGE_NAME = 'thaott084/user-service'
    CONTAINER_NAME = 'user-service'
    PORT = '3000'
  }

  stages {
    stage('Clone source') {
      steps {
        git branch: 'main', url: 'git@github.com:ThuThaoB23/microservice-blog.git'
      }
    }

    stage('Set Docker tag') {
      steps {
        script {
          def shortCommit = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
          env.IMAGE_TAG = "build-${BUILD_NUMBER}-${shortCommit}"
        }
      }
    }

    stage('Build Docker image') {
      steps {
        dir('user-service') {
          sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
        }
      }
    }

    stage('Push Docker image') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'docker-hub-cred',
          usernameVariable: 'thaott084',
          passwordVariable: 'Dangthuc02@'
        )]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push $IMAGE_NAME:$IMAGE_TAG
          '''
        }
      }
    }

    stage('Deploy by Docker Compose') {
      steps {
        script {
          // Inject tag vào docker-compose.yml (nếu có dùng image tag động)
          sh """
            sed -i 's|image: $IMAGE_NAME:.*|image: $IMAGE_NAME:$IMAGE_TAG|' docker-compose.yml
          """
        }

        sh '''
          docker compose down || true
          docker compose pull
          docker compose up -d
        '''
      }
    }
  }

  post {
    success {
      echo "✅ CI/CD thành công: $IMAGE_NAME:$IMAGE_TAG đã được deploy!"
    }
    failure {
      echo "❌ CI/CD thất bại. Kiểm tra lại các bước."
    }
  }
}
