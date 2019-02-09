pipeline {
  agent any
  tools {nodejs "NodeJS11"}
  stages {

    stage("setup") {
      steps {
        sh label: 'setup', script: 'npm ci'
      }
    }

    stage("tests") {
      steps {
        sh label: 'test', script: 'npm run test'
      }
    }

    stage("build") {
      steps {
        sh label: 'build', script: 'npm run build'
      }
    }

    stage("deploy") {
      steps {
        sh label: 'deploy', script: 'npm run deploy'
      }
    }

  }
}

