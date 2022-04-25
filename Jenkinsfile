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
                          sh 'npm install'
                     }
                }
                stage('Unit tests and coverage') {
                     steps {
                          sh 'npm run test:coverage'
                     }
                     //post {
                     //    always {
                             // cobertura coberturaReportFile: 'coverage/cobertura-coverage.xml', lineCoverageTargets: '70, 70, 0'
                    //     }
                   // }
                }
                stage('Build') {
                     steps {
                          sh 'npm run build'
                          zip zipFile: 'build.zip', archive: true, dir: 'bmi-calculator/build'
                     }
                }
           }
        }
    }
}
