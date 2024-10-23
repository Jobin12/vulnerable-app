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
        script {
          sh 'snyk code test --sarif'
        }
      }
    }

  }
}