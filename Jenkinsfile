pipeline {
  agent any
  environment {
    FRONTEND_GIT = 'https://github.com/propropro1905/tutorial-jenkins-frontend.git'
    FRONTEND_BRANCH = 'master'
    FRONTEND_IMAGE = 'pendragon1902/testapp'

  }
  stages {
    stage('Build JS') {
      agent {
        docker {
          image 'node:latest'
          args '-v testapp_modules:$WORKSPACE/node_modules'
        }
      }
      steps {
        git(url: FRONTEND_GIT, branch: FRONTEND_BRANCH)
        sh 'npm i'
        sh 'npm run build'
        stash(name: 'frontend', includes: 'build/*/**')
      }
    }
    stage('Build Image') {
      steps {
        unstash 'frontend'
        script {
          docker.withRegistry('', 'docker-hub') {
            def image = docker.build(FRONTEND_IMAGE)
            image.push("latest")
          }
        }
      }
    }
  }
}
