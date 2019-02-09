pipeline {
  stages {
    nodejs('NodeJS11') {

      stage("pull") {
        echo "pulling..."
          git "https://github.com/fabientownsend/jenkins-test.git"

          echo "setup"
          sh label: 'setup', script: 'npm ci'
      }

      stage("tests") {
        echo "testing..."
          sh label: 'test', script: 'npm run test'
      }

      stage("build") {
        echo "building..."
          sh label: 'build', script: 'npm run build'
      }

      stage("deploy") {
        echo "deploying"
          sh label: 'deploy', script: 'npm run deploy'
      }

    }
  }
}

