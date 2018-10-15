pipeline {
  agent any
  tools {nodejs "nodejs10.12"}

  stages {

    stage('Cloning Git repository') {
      steps {
        git 'https://github.com/VivienZo/express-hello.git'
      }
    }

    stage('SonarQube analysis') {
      steps {
        echo 'Mock SonarQube analysis'
      }
    }

    stage('Install dependencies') {
      steps {
        sh 'cd api'
        sh 'npm install'
        sh 'cd ../frontend'
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'cd api'
        sh 'npm test'
      }
    }

  }
}