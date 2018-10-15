pipeline {
  agent any
  tools {nodejs "nodejs10.12"}

  stages {

    stage('Cloning Git') {
      steps {
        git 'https://github.com/VivienZo/express-hello.git'
      }
    }

    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

  }
}
