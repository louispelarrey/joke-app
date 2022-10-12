pipeline {
  agent any

  stages {
    stage('build') {
      steps {
        nodejs(nodeJSInstallationName: 'nodejs')
        sh "npm install"
        sh "npm run build"
        sh "npm test"
      }
    }
  }
}