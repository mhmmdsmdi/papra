pipeline {
  agent any

  tools {
        go 'go' // Name from Global Tool Configuration
        docker 'docker'
    }

  environment {
    COMPOSE_PROJECT_NAME = 'papra'
    COMPOSE_FILE = 'docker-compose.yml'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build and Run with Docker Compose') {
      steps {
        script {
          sh 'docker-compose up --build -d'
        }
      }
    }

    stage('Health Check') {
      steps {
        script {
          // Wait for app to start (basic delay or better with curl)
          sh 'sleep 10'
          sh 'curl --fail http://localhost:1221 || exit 1'
        }
      }
    }
  }

  post {
    always {
      echo "Cleaning up containers..."
      sh 'docker-compose down -v'
    }
  }
}
