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

    stage('Test') {
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

  }
  environment {
    registry = 's312r365fdh232345kklh34256sd76/ci-cd-practical-task-ali-palitaev'
  }
}