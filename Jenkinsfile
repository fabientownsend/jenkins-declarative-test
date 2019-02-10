#!/usr/bin/groovy

pipeline {
  agent any
  tools {nodejs "NodeJS11"}
  stages {

    stage("build") {
      steps {
        sh """
          npm ci
        """
      }
    }

    stage("tests") { 
      parallel {
        stage("qa: static code analysis") { 
          steps {
            sh """
              echo "qa: static code analysis"
            """
          }
        }

        stage("qa: unit & integration tests") {
          steps {
            sh label: 'test', script: 'npm run test'
          }
        }
      }
    }

    stage('Optional Steps: deploy to dev') {
      steps { deployTo('dev') }
    }

    stage('approval: stage') {
      steps {
        script {
          timeout(time: 10, unit: 'SECONDS') {
            input "Deploy to stage?"
          }
        }
      }
    }

    stage("package: stage") {
      steps {
        sh label: 'build', script: 'npm run build'
      }
    }


    stage("deploy: stage") {
      steps {
        sh label: 'deploy', script: 'npm run deploy'
      }
    }

    // could to the e2e testing on stage
  }
}

def deployTo(environment) { // it does not need script {} as it's an external function
  def shouldDeploy = input(
      id: 'userInput', message: "Deploy to ${environment}?", parameters: [
        [
          $class: 'BooleanParameterDefinition',
          defaultValue: 'true',
          description: 'Environment',
          name: "deploy to ${environment}"
        ]
      ]
  )

  if (shouldDeploy) {
    sh("echo would call ./deploy-${environment}.sh")
      // sh('./deploy.sh')
  } else {
    sh('echo Do nothing')
  }
}
