def app

pipeline {
  agent none

  environment {
    IMAGE_NAME = "nursultanmakhambet/cicd-pipeline"
  }

  stages {

    stage('Checkout') {
      agent any
      steps {
        checkout scm
      }
    }

    stage('Build') {
      agent {
        docker {
          image 'node:18'
        }
      }
      steps {
        sh 'chmod +x scripts/build.sh && ./scripts/build.sh'
      }
    }

    stage('Tests') {
      agent {
        docker {
          image 'node:18'
        }
      }
      steps {
        sh 'chmod +x scripts/test.sh && ./scripts/test.sh'
      }
    }

    stage('Docker Build') {
      agent any
      steps {
        script {
          app = docker.build(IMAGE_NAME)
        }
      }
    }

    stage('Docker Push') {
      agent any
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

