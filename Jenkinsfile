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
      steps {
        deployToDev()
      }
    }

    stage('approval: stage') {
      steps {
        script {
          timeout(5) {
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
  }
}

def deployToDev() {
  env.RELEASE_SCOPE = input(
      id: 'userInput', message: 'Deploy to dev?', parameters: [
      [
      $class: 'BooleanParameterDefinition',
      defaultValue: 'true',
      description: 'Environment',
      name: 'deploy to dev']
      ]
  )

  if (env.RELEASE_SCOPE) {
    sh('echo Do nothing')
  } else {
    sh('echo would call ./deploy.sh')
      // sh('./deploy.sh')
  }
}
