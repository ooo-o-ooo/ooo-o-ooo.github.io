pipeline {
  agent any
  stages {
    stage('Node.js') {
      steps {
        sh 'rm -rf /usr/lib/node_modules/npm/'
        dir('/root/.cache/downloads') {
          sh 'wget -nc "https://coding-public-generic.pkg.coding.net/public/downloads/node-linux-x64.tar.xz?version=v16.13.0" -O node-v16.13.0-linux-x64.tar.xz | true'
          sh 'tar -xf node-v16.13.0-linux-x64.tar.xz -C /usr --strip-components 1'
        }

        sh 'node -v'
      }
    }
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
          sh 'npm install -g hexo-cli'
          sh 'hexo g'
        }
      }
      stage('阶段 4-1') {
        steps {
          sh 'npm install hexo-deployer-git --save'
          sh 'hexo d'
        }
      }
    }
  }