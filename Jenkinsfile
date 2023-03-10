pipeline {
  agent any
  stages {
    stage('github') {
      steps {
        git(branch: 'main', credentialsId: 'a3e14212-7f76-4abe-98bd-3d0af0423756', url: 'https://github.com/AmitAaranya/CPP.git')
      }
    }

    stage('GIT-pull-wxi') {
      steps {
        dir(path: "${env.WORKSPACE}/wixMSI/bin") {
          bat 'git init'
          bat "git remote add origin${BUILD_NUMBER} https://github.com/AmitAaranya/CPP-Output.git"
        }

      }
    }

    stage('Build Stage') {
      parallel {
        stage('MSBuild1') {
          steps {
            bat "docker run -v ${WORKSPACE}:C:\\app msbuild msbuild c:\\app\\Demowix\\Demowix.vcxproj"
          }
        }

        stage('MSBuild2') {
          steps {
            bat 'docker run msbuild'
          }
        }

        stage('MSBuild3') {
          steps {
            sleep 5
          }
        }

      }
    }

    stage('WixBuild') {
      parallel {
        stage('WixBuild') {
          steps {
            bat 'powershell -command ls'
          }
        }

        stage('Wix2') {
          steps {
            sleep 5
          }
        }

        stage('Wix3') {
          steps {
            sleep 5
          }
        }

      }
    }

    stage('GIT-push') {
      steps {
        bat 'git config --global user.email "amitaaranya@gmail.com"'
        bat 'git config --global user.name "Amit Aaranya"'
        dir(path: "${env.WORKSPACE}/wixMSI/bin") {
          bat "git pull origin${BUILD_NUMBER} master"
          bat 'git add .'
          bat 'git commit -m "New MSI build push"'
          bat "git push --set-upstream origin${BUILD_NUMBER} master"
          bat "git remote rm origin${BUILD_NUMBER}"
        }

      }
    }

  }
  post {
    always {
      echo 'Clearing the workspace'
      deleteDir()
      dir("${workspace}@tmp") {
        deleteDir()
      }

    }

  }
}