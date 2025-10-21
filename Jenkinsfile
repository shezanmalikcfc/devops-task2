pipeline {
  agent any

  triggers {
    // Simple auto-check: polls Git every ~2 minutes (easier than webhooks for beginners)
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
        // On a Windows agent, use 'bat'; on Linux agent use 'sh'
        bat 'docker --version'
        bat 'docker build -t jenkins-demo:latest .'
      }
    }

    stage("Test") {
      steps {
        // We just run the npm test inside a temporary container
        bat 'docker run --rm jenkins-demo:latest npm test'
      }
    }

    stage("Deploy (Run Container)") {
      steps {
        script {
          // Stop/remove any existing container with the same name
          bat 'for /f "tokens=*" %i in (\'docker ps -aq -f "name=jenkins-demo"\') do docker stop %i'
          bat 'for /f "tokens=*" %i in (\'docker ps -aq -f "name=jenkins-demo"\') do docker rm %i'
          // Run the app on host port 8081
          bat 'docker run -d -p 8081:8080 --name jenkins-demo jenkins-demo:latest'
        }
      }
    }
  }

  post {
    always {
      // Create a small build artifact file
      bat 'mkdir build 2>nul & echo Build at %DATE% %TIME% > build\\info.txt'
      archiveArtifacts artifacts: 'build/**', fingerprint: true
    }
  }
}
