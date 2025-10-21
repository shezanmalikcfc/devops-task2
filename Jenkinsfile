pipeline {
  agent any
  triggers { pollSCM("H/2 * * * *") }

  stages {
    stage("Checkout") {
      steps { checkout scm }
    }

    stage("Build Docker Image") {
      steps {
        sh "docker --version"
        sh "docker build -t jenkins-demo:latest ."
      }
    }

    stage("Test") {
      steps {
        sh "docker run --rm jenkins-demo:latest npm test"
      }
    }

    stage("Deploy (Run Container)") {
      steps {
        sh "docker ps -aq -f name=jenkins-demo | xargs -r docker stop || true"
        sh "docker ps -aq -f name=jenkins-demo | xargs -r docker rm || true"
        sh "docker run -d -p 8081:8080 --name jenkins-demo jenkins-demo:latest"
      }
    }
  }

  post {
    always {
      sh "mkdir -p build && echo 'Build at $(date)' > build/info.txt"
      archiveArtifacts artifacts: "build/**", fingerprint: true
    }
  }
}
