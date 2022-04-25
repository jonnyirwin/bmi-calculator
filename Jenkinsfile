pipeline {
    agent none
    stages {
        stage('Continuous Integration') {
           
           stages {
                stage('Install dependencies') {
                     agent {
                         docker {
                              image 'node:16.13.1-alpine'
                         }
                    }
                     steps {
                          sh 'npm install'
                     }
                }
                stage('Unit tests and coverage') {
                     agent {
                         docker {
                              image 'node:16.13.1-alpine'
                         }
                     }
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
                     agent {
                         docker {
                              image 'node:16.13.1-alpine'
                         }
                     }
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
                     agent {
                          docker {
                               image 'docker'
                          }
                     }
                     steps {
                          script {
                            if (fileExists("build.zip")) {
                                sh "rm -f build.zip"
                            }
                          }
                          unstash name: 'buildZip'
                          unzip zipFile: 'build.zip', dir: 'dest'
                          script {
                               docker.build('jonnyirwin/bmi-calc')
                          }
                     }
                }
           }
        }
    }
}
