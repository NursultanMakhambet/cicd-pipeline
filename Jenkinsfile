def app

pipeline {
  agent {
    docker {
      image 'node18-docker'
      args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
  }

  environment {
    IMAGE_NAME = "nursultanmakhambet/cicd-pipeline"
  }

  stages {

    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build') {
      steps { sh 'chmod +x scripts/build.sh && ./scripts/build.sh' }
    }

    stage('Tests') {
      steps { sh 'chmod +x scripts/test.sh && ./scripts/test.sh' }
    }

    stage('Docker Build') {
      steps {
        script {
          app = docker.build(IMAGE_NAME)
        }
      }
    }

    stage('Docker Push') {
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

