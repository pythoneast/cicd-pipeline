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

  }
  environment {
    registry = 's312r365fdh232345kklh34256sd76/ci-cd-practical-task-ali-palitaev'
  }
}