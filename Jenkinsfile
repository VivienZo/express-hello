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
        echo 'Install backend dependencies'
        sh 'cd api; npm install'
        echo 'Install frontend dependencies'
        sh 'cd frontend; npm install'
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
