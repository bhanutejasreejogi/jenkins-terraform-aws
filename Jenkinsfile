pipeline {
  agent any

  environment {
    AWS_DEFAULT_REGION = "us-east-1"
  }

  stages {

    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('Terraform Init') {
      steps {
        dir('terraform') {
          bat 'terraform init'
        }
      }
    }

    stage('Terraform Apply') {
       options {
           timeout(time: 10, unit: 'MINUTES')
       }
      steps {
        withCredentials([[
          $class: 'AmazonWebServicesCredentialsBinding',
          credentialsId: 'aws-creds'
        ]]) {
          dir('terraform') {
            bat 'terraform apply -auto-approve -input=false -lock-timeout=5m -var="bucket_name=bhanu-cicd-demo-12345"'
          }
        }
      }
    }
  }
}


