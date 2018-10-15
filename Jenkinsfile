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
    
    stage('Dependencies security audit') {
      steps {
        echo '===== Analyze backend dependencies ====='
        sh 'cd api; npm audit'
        echo '===== Analyze frontend dependencies ====='
        sh 'cd frontend; npm audit'
      }
    }

    stage('Install dependencies') {
      steps {
        echo '===== Install backend dependencies ====='
        sh 'cd api; npm install'
        echo '===== Install frontend dependencies ====='
        sh 'cd frontend; npm install'
      }
    }

    stage('Test') {
      steps {
        echo '===== Test backend ====='
        sh 'cd api; npm test'
        echo '===== Test frontend ====='
        sh 'cd frontend; npm test'
      }
    }
    
    stage('Build backend') {
      steps {
        sh 'cd api; npm run clean'
        echo '===== Upload API to S3 ====='
        sh 'aws s3 sync ./api/ s3://tms-dev-london-back-end --delete'
        echo '===== Start blue green deployment ====='
        sh 'aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names TMS-dev-london-BEAutoscalingGroup-1XOZAQS5KHFXA --query AutoScalingGroups[].Instances[].InstanceId --output text'
      }
    }

  }
}
