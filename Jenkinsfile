pipeline {
    agent{
        docker {
            image 'node:16.13.1-alpine'
        }
    }
    stages {
        stage('Continuous Integration') {
           stages {
                stage('Install dependencies') {
                     steps {
                          echo 'Installing dependencies'
                          sh 'npm install'
                     }
                }
                stage('Unit tests') {
                     steps {
                          echo 'Testing'
                          sh 'npm test'
                     }
                }
                stage('Build') {
                     steps {
                          echo 'Building'
                     }
                }
           }
        }
    }
}