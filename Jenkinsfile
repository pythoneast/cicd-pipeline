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

    stage('Docker Image Build') {
      steps {
        script {
          sh "docker build -t ${registry}:${BUILD_NUMBER} ."
          sh "docker tag ${registry}:${BUILD_NUMBER} ${registry}:latest "
          echo 'Docker Image Build Is Completed'
        }

      }
    }

    stage('Check Docker Image for Vulnerabilities') {
      steps {
        script {
          sh 'mkdir -p reports'
          sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl'
          def vulnerabilities = sh(script: "trivy image --ignore-unfixed --exit-code 0 --severity CRITICAL,HIGH,MEDIUM --format template --template '@html.tpl' -o reports/image-scan.html --no-progress ${registry}:${env.BUILD_ID}", returnStdout: true).trim()
          echo "Vulnerability Report:\n${vulnerabilities}"
          publishHTML target : [
            allowMissing: true,
            alwaysLinkToLastBuild: true,
            keepAll: true,
            reportDir: 'reports',
            reportFiles: 'image-scan.html',
            reportName: 'Trivy Scan',
            reportTitles: 'Trivy Scan'
          ]
        }

      }
    }

    stage('Publish Docker Image') {
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