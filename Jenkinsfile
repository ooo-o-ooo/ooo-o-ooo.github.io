pipeline {
  agent any
  stages {
    stage('阶段 1-1') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: env.GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: env.GIT_REPO_URL,
            credentialsId: env.CREDENTIALS_ID
          ]]])
        }
      }
      stage('阶段 2-1') {
        steps {
          sh '''echo hello CODING
npm install -g hexo-cli
hexo g
hexo d'''
        }
      }
    }
  }