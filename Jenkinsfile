pipeline {
    agent none
    stages {
        stage('Continuous Integration') {
           agent {
                docker {
                     image 'node:16.13.1-alpine'
                }
           }
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
                          script {
                            if (fileExists("build")) {
                                sh "rm -rf build"
                            }
                          }
                          script {
                            if (fileExists("build.zip")) {
                                sh "rm -f build.zip"
                            }
                          }
                          sh 'npm run build'
                          zip zipFile: 'build.zip', archive: true, dir: 'build', overwrite: true
                          stash name: 'buildZip', includes: 'build.zip', allowEmpty: false
                     }
                }
                stage('Docker Image build') {
                     steps {
                          script {
                            if (fileExists("build.zip")) {
                                sh "rm -f build.zip"
                            }
                          }
                          unstash name: 'buildZip'
                          unzip zipFile: 'build.zip', dir: 'dest'
                          docker.build('jonnyirwin/bmi-calc')
                     }
                }
           }
        }
    }
}
