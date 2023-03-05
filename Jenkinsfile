pipeline {
  agent any
  stages {
     stage("Parameter Setup") {
      steps {
        script {
        properties([
          parameters([
            choice(choices: ['apply', 'destroy'], name: 'ACTION')])])
        }
      }
    }
    stage("Terraform Init") {
      steps {
        script {
          dir('azure/') {
            sh 'terraform init'
          }
        }
      }
    }
    stage("Terraform Validate") {
      steps {
        script {
          dir('azure/') {
            sh 'terraform validate'
          }
        }
      }
    }
    stage('Terraform Plan') {
      steps {
        script {
          dir('azure/') {
            sh 'terraform plan'
          }
        }
      }
    }
    stage('Terraform Approval') {
      steps {
        script {
          def userInput = input(id: 'confirm', message: 'Approve Terraform ?', parameters: [
            [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Approve Terraform', name: 'confirm']
          ])
        }
      }
    }
    stage('Terraform Action') {
      steps {
        script {
          dir('azure/') {
            sh 'terraform $ACTION --auto-approve'
          }
        }
      }
    }
  }
}