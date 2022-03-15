pipeline {
    agent {
        any {
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B clean install' 
            }
        }
        stage('Xray Scan'){
            steps {
                script {
                    xrayConfig = [
                        'buildName'   : buildInfo.name,
                        'buildNumber' : buildInfo.number,
                        'failBuild'   : "${params.FAIL_BUILD}".toBoolean()
                    ]
                    xrayResults = rtServer.xrayScan xrayConfig
                    echo xrayResults as String
                }
            }
        }
    }
}
