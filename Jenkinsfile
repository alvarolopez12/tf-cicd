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
    stage('Checkov') {
        steps {
            checkout([$class: 'GitSCM', branches: [[name: 'master']], userRemoteConfigs: [[url: 'https://github.com/alvarolopez12/tf-cicd']]])
            script { 
                sh """pipenv install
                pipenv run pip install bridgecrew
                pipenv run bridgecrew --directory . --bc-api-key 69a43622-8d7a-48e1-b3d2-92efe6f10dc6 --repo-id alvarolopez12/tf-cicd"""
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
