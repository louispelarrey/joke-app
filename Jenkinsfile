pipeline {
  agent any

  parameters {
    choice(name: 'NODE_VERSION', choices: ['18', '16'])
  }

  environment {
    tag = "registry.heroku.com/test-cicd123soleil/web"
  }

  stages {
    stage('build') {
      steps {
        script {
          try {
            nodejs(nodeJSInstallationName : "${params.NODE_VERSION}"){
              sh "npm install"
              sh "npm run build"
              sh "npm test"
            }
          } catch (Exception e) {
            throw e
          }
        }
      }
    }

    stage('deploy') {
      steps {
        script{
          docker.withRegistry('https://registry.heroku.com', 'heroku-id') {
            def image = docker.build("${env.tag}")
            image.push()
          }
        }
      }
    }
  }
}