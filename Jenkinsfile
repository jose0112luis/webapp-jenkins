pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "jose0112luis/webapp:${env.BUILD_NUMBER}"
  }

  stages {
    stage('Build') {
      steps {
        script {
          dockerImage = docker.build "${env.ARTIFACT_ID}"
        }
      }
    }

    stage('Run tests') {
      steps {
        sh "docker run ${dockerImage.id} npm test"
      }
    }

    stage('Publish') {
      when {
        branch 'master'
      }
      steps {
        script {
          docker.withRegistry("", "DockerHubCredentialsTest") {
            dockerImage.push()
          }
        }
      }
    }

    // stage('Schedule Staging Deployment') {
    //   when {
    //     branch 'master'
    //   }
    //   steps {
    //     build job: 'deploy-webapp-staging', parameters: [string(name: 'ARTIFACT_ID', value: "${env.ARTIFACT_ID}")], wait: false
    //   }
    // }
  }
}
