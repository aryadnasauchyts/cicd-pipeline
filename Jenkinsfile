pipeline {
  agent {
    docker {
      image 'node:latest'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Git checkout') {
      steps {
        script {
          checkout scm
        }

      }
    }

    stage('Application Build') {
      steps {
        script {
          sh 'chmod +x scripts/build.sh'
          sh 'scripts/build.sh'
        }

      }
    }

    stage('Tests') {
      steps {
        script {
          sh 'scripts/test.sh'
        }

      }
    }

    stage('Docker Image Build') {
      steps {
        script {
          docker.build("${registry}:${env.Build_ID}")
        }

      }
    }

    stage('Docker Image Push') {
      steps {
        script {
          docker.withRegistry('','dockerhub_id'){
            docker.image("${registry}:${env.BUILD_ID}").push('latest')
            docker.image("${registry}:${env.BUILD_ID}").push("${env.BUILD_ID}")
          }
        }

      }
    }

  }
  environment {
    registry = 'asauchyts/cicd-task'
  }
}