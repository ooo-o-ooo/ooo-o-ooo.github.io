pipeline {
  agent any
  stages {
    stage('更新Nodejs版本') {
      steps {
        sh 'rm -rf /usr/lib/node_modules/npm/'
        dir('/root/.cache/downloads') {
          sh 'wget -nc "https://coding-public-generic.pkg.coding.net/public/downloads/node-linux-x64.tar.xz?version=v16.13.0" -O node-v16.13.0-linux-x64.tar.xz | true'
          sh 'tar -xf node-v16.13.0-linux-x64.tar.xz -C /usr --strip-components 1'
        }

        sh 'node -v'
      }
    }
    stage('从代码仓库检出') {
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
      stage('安装npm执行环境') {
        steps {
          sh 'npm install -g hexo-cli'
          sh 'npm install hexo-deployer-git --save'
        }
      }
      stage('Hexo生成并推送') {
        steps {
          sh 'hexo g'
          sh 'hexo d'
        }
      }
    }
  }