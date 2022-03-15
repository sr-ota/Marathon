def rtServer, buildInfo

pipeline {
    agent {
        any {
        }
    }
    parameters {
        string (name: 'ART_URL', defaultValue: 'http://localhost:8081/artifactory', description: 'Artifactory where artifacts will be deployed/resolved')
        string (name: 'ART_USER', defaultValue: 'admin', description: 'Artifactory user for deploy/resolve artifacts')
        string (name: 'ART_PASSWORD', defaultValue: 'password', description: 'Artifactory password for deploy/resolve artifacts')
    }
    stages {
        stage('Setup'){
            steps {
                script{
                    rtServer = Artifactory.newServer url: "${params.ART_URL}", username: "${params.ART_USER}", password: "${params.ART_PASSWORD}"
                }
            }
        }
        stage('Build') { 
            steps {
                sh 'mvn -B clean install' 
            }
        }
        stage('Xray Scan'){
            steps {
                script {
                    xrayConfig = [
                        'failBuild'   : "${params.FAIL_BUILD}".toBoolean()
                    ]
                    xrayResults = rtServer.xrayScan xrayConfig
                    echo xrayResults as String
                }
            }
        }
    }
}
