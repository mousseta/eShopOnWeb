pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'dotnet build eShopOnWeb.sln'
      }
    }

    stage('test') {
      parallel {
        stage('unit') {
          steps {
            sh 'dotnet test tests/UnitTests'
          }
        }

        stage('integration') {
          steps {
            sh 'dotnet test tests/FunctionalTests'
          }
        }

        stage('functional') {
          steps {
            warnError(message: 'functional problems') {
              sh 'dotnet test tests/FunctionalTests'
            }

          }
        }

      }
    }

    stage('deployment') {
      steps {
        sh 'dotnet publish eShopOnWeb.sln -o /var/aspnet'
        dir(path: '/var/aspnet') {
          archiveArtifacts(onlyIfSuccessful: true, artifacts: '*')
        }

      }
    }

  }
}