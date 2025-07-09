pipeline {
  agent {
    docker {
      image 'docker:24.0.7-cli' // Docker CLI (không phải dind)
      args  '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  environment {
    IMAGE_NAME = 'thaott084/user-service'
    GIT_COMMIT_SHORT = "${env.GIT_COMMIT[0..6]}"
    BUILD_TAG = "build-${env.BUILD_NUMBER}-${env.GIT_COMMIT_SHORT}"
    CONTAINER_NAME = 'user-service'
    PORT = '3000'
  }

  stages {
    stage('Clone code') {
      steps {
        git branch: 'main', url: 'https://github.com/ThuThaoB23/microservice-blog.git'
      }
    }

    stage('Build Docker image') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$BUILD_TAG ./user-service'
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
            docker push $IMAGE_NAME:$BUILD_TAG
          '''
        }
      }
    }

    stage('Deploy via Docker Compose') {
      steps {
        sh '''
          cd user-service/deploy
          sed -i "s|image: .*|image: $IMAGE_NAME:$BUILD_TAG|g" docker-compose.yml
          docker compose -f docker-compose.yml --env-file /root/env-app/.env up -d --remove-orphans
        '''
      }
    }
  }

  post {
    success {
      echo "✅ Build & deploy thành công: $IMAGE_NAME:$BUILD_TAG"
    }
    failure {
      echo "❌ Có lỗi xảy ra khi build/deploy"
    }
  }
}
