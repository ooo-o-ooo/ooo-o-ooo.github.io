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
      parallel {
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
          stage('阶段 2-2') {
            steps {
              sh 'echo $GIT_DEPLOY_KEY'
            }
          }
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
          sh 'rm -rf .deploy_git'
          sh 'hexo g'
        }
      }
    }
  }