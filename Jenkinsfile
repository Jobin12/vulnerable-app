pipeline {
  agent any

  stages {

    stage('Git Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Jobin12/vulnerable-app.git'
      }
    }

    stage ('AWS CodeGuru Security') {
      steps {
        script {
          sh """
            ./run_codeguru_security.sh vulnerable-app . us-east-1
          """
        }
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
            recordIssues tool: sarif(id: 'SNYK', pattern: 'synk_report.sarif')
          }
        }
      }
    }

    stage('SonarQube Scan') {
      steps {
       withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]) {
        script {
          sh """
            export SONAR_TOKEN=${env.SONAR_TOKEN}
            mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=vulnerable-app_vulnerable-app
          """
        }
       } 
      }
    }
  }
}