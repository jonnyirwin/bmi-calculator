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
                stage('Unit tests and coverage') {
                     steps {
                          echo 'Testing'
                          sh 'npm run test:coverage'
                     }
                     post {
                         always {
                              cobertura coberturaReportFile: 'coverage/cobertura-coverage.xml', lineCoverageTargets: '70, 70, 0'
                         }
                    }
                }
                stage('Build') {
                     steps {
                          echo 'Building'
                          sh 'npm run build'
                     }
                }
           }
        }
    }
}
