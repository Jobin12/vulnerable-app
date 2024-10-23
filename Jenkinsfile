pipeline {
  agent any

  stages {

    stage('Git Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Jobin12/vulnerable-app.git'
      }
    }

    stage('Snyk DeepCode') {
      steps {
        withCredentials([string(credentialsId: 'snyk_token', variable: 'SNYK_TOKEN')]) {
          script {
            sh """
              snyk auth ${env.SNYK_TOKEN}
              snyk code test --sarif-file-output=synk_report.sarif || echo 'Issues Found'
              """
            withWarnings {
              scanForWarnings 'synk_report.sarif'
            }
          }
        }
      }
    }

  }
}