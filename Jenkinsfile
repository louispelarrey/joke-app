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
            sh "docker buildx --platform linux/adm64 -t) ${tag}:latest -t ${tag}:${BUILD_ID} ."
            def image = docker.build("${env.tag}")
            image.push()

            sh "docker push ${tag}:latest"
          }
        }

        withCredentials([usernamePassword(credentialsId: 'heroku-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh "HEROKU_API_KEY=${PASSWORD} npx heroku container:release web --app=test-cicd123soleil"
        }
      }
    }
  } 
}