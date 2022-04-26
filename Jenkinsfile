pipeline {
    agent any
    stages {
        stage('Continuous Integration') {
          agent {
               docker {
                    image 'node:16-alpine'
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
                
           }
        }
        stage('Docker Image build') {
               steps {
                    sh 'id $(whoami)'
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
                    withDockerRegistry([ credentialsId: "hub.docker.com", url: "" ]) {
                         sh 'docker push jonnyirwin/bmi-calc'
                    }
               }
          }
        stage('Trivy image scan') {
             agent {
                  docker {
                       image 'aquasec/trivy:latest'
                       args '--entrypoint=""'
                  }
             }
             steps {
                  sh 'trivy image jonnyirwin/bmi-calc'
             }        
        }
     }
}
