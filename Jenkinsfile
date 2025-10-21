pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // add your test command, example:
                // sh 'npm test' or 'pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the app...'
                sh 'docker run -d -p 8081:8080 myapp:latest'
            }
        }
    }
}
