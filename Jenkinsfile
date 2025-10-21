pipeline {
  agent any

  triggers {
    // Polls Git every ~2 minutes; simplest auto-trigger
    pollSCM("H/2 * * * *")
  }

  stages {
    stage("Checkout") {
      steps {
        checkout scm
      }
    }

    stage("Build Docker Image") {
      steps {
        sh 'docker --version'
        sh 'docker build -t jenkins-demo:latest .'
      }
    }

    stage("Test") {
      steps {
        // run npm test inside the built image
        sh 'docker run --rm jenkins-demo:latest npm test'
      }
    }

    stage("Deploy (Run Container)") {
      steps {
        // stop + remove any previous container named jenkins-demo (ignore errors if none)
        sh 'docker ps -aq -f name=jenkins-demo | xargs -r docker stop || true'
        sh 'docker ps -aq -f name=jenkins-demo | xargs -r docker rm || true'
        // run new container on host port 8081
        sh 'docker run -d -p 8081:8080 --name jenkins-demo jenkins-demo:latest'
      }
    }
  }

  post {
    always {
      // create an artifact to archive
      sh 'mkdir -p build && echo "Build at $(date)" > build/info.txt'
      archiveArtifacts artifacts: 'build/**', fingerprint: true
    }
  }
}
