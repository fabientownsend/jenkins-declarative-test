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

    stage('approval: dev') {
      steps {
        script {
          timeout(5) {
            input "Deploy to dev?"
          }
        }
      }
    }

    stage("package: dev") {
      steps {
        sh label: 'build', script: 'npm run build'
      }
    }


    stage("deploy: dev") {
      steps {
        sh label: 'deploy', script: 'npm run deploy'
      }
    }
  }
  post {
    always {
      currentBuild.result = 'SUCCESS'
    }
  }
}

