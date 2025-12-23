pipeline {
  agent any

  environment {
    IMAGE_NAME = "nursultanmakhambet/cicd-pipeline"
  }

  stages {
    stage('Git Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Application Build') {
      steps {
        sh 'chmod +x scripts/build.sh && ./scripts/build.sh'
      }
    }

    stage('Tests') {
      steps {
        sh 'chmod +x scripts/test.sh && ./scripts/test.sh'
      }
    }

    stage('Docker Image Build') {
      steps {
        script {
          app = docker.build("${IMAGE_NAME}")
        }
      }
    }

    stage('Docker Image Push') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_creds_id') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
          }
        }
      }
    }
  }
}
