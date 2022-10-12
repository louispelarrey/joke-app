pipeline {
  agent any

  parameters {
    choice(name: 'NODE_VERSION', choices: ['18', '16'])
  }

  stages {
    stage('build') {
      steps {
        nodejs(nodeJSInstallationName: 'nodejs':"$
        {params.NODE_VERSION}"){
          sh "npm install"
          sh "npm run build"
          sh "npm test"
        }
      }
    }
  }
}