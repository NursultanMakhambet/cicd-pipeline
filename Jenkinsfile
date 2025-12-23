def app

pipeline {
  agent {
    docker {
      image 'node:18'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  environment {
    IMAGE_NAME = "nursultanmakhambet/cicd-pipeline"
  }

  stages {
    stage('Git Checkout') {
      steps { checkout scm }
    }

    stage('Application Build') {
      steps { sh 'chmod +x scripts/build.sh && ./scripts/build.sh' }
    }

    stage('Tests') {
      steps { sh 'chmod +x scripts/test.sh && ./scripts/test.sh' }
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
          docker.withRegistry('https://index.docker.io/v1/', 'docker_hub_creds_id') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
          }
        }
      }
    }
  }
}

