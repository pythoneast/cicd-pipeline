pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      steps {
        script {
          git branch: 'main', credentialsId: 'github-id', url: 'https://github.com/pythoneast/cicd-pipeline.git'
          echo 'Git Checkout Completed'
        }

      }
    }

    stage('Application Build') {
      steps {
        script {
          sh 'chmod +x ./scripts/build.sh'
          sh './scripts/build.sh'
          echo 'Application Build Is Completed'
        }

      }
    }

    stage('Tests') {
      post {
        success {
          echo 'Test step is passed'
        }

        failure {
          echo 'Test step is failed'
        }

      }
      steps {
        script {
          sh 'chmod +x ./scripts/test.sh'
          sh './scripts/test.sh'
        }

      }
    }

    stage('Docker Image Build') {
      steps {
        script {
          sh "docker build -t ${registry}:${BUILD_NUMBER} ."
          sh "docker tag ${registry}:${BUILD_NUMBER} ${registry}:latest "
          echo 'Docker Image Build Is Completed'
        }

      }
    }

    stage('Docker Image Push') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub_id') {
            docker.image("${registry}:${env.BUILD_NUMBER}").push('latest')
            docker.image("${registry}:${env.BUILD_NUMBER}").push("${env.BUILD_NUMBER}")
            echo 'Docker Image Push to DockerHub Completed'
          }
        }

      }
    }

  }
  environment {
    registry = 's312r365fdh232345kklh34256sd76/ci-cd-practical-task-ali-palitaev'
  }
}