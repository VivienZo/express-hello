pipeline {
  agent any
  tools {nodejs "nodejs"}
  environment {
    InstanceID = """${sh(
        returnStdout: true,
        script: '/home/jenkins/.local/bin/aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names TMS-dev-london-BEAutoscalingGroup-1XOZAQS5KHFXA --query AutoScalingGroups[].Instances[].InstanceId --output text'
      )}"""
  }

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
        sh 'cd frontend; npm run lint'
        sh 'cd frontend; npm test'
      }
    }
    
    stage('Build') {
      steps {
        sh 'cd frontend; npm run build'
      }
    }
    
    stage('Deploy') {
      steps {
        sh 'cd api; npm run clean'
        echo '===== Upload API to S3 ====='
        sh '/home/jenkins/.local/bin/aws s3 sync ./api/ s3://tms-dev-london-back-end --delete'
        sh '/home/jenkins/.local/bin/aws s3 sync ./frontend/dist/ s3://tms-dev-london-front-end --delete'
        echo '===== Restart EC2 instances ====='
        sh "/home/jenkins/.local/bin/aws ec2 terminate-instances --instance-ids ${InstanceID}"
      }
    }

  }
}
