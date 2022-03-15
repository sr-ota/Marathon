def rtServer, buildInfo

pipeline {
    agent {
        any {
        }
    }
    parameters {
        string (name: 'ART_API_KEY', defaultValue: '', description: 'API Key for JFrog Artifactory')
        string (name: 'ART_URL', defaultValue: '', description: 'URL for JFrog Artifactory')
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B clean install' 
                sh 'ls -al'
                sh 'ls -al target'
            }
        }
        stage('Xray Scan'){
            steps {
                sh 'jf rt upload --url ${ART_URL} --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/marathon.jar marathon-web/'
           }
        }
    }
}
