def rtServer, buildInfo

pipeline {
    agent {
        any {
        }
    }
    parameters {
        string (name: 'ART_URL', defaultValue: 'https://evaluate.jfrog.io/artifactory', description: 'URL for JFrog Artifactory')
    }
    environment {
        CI = true
        ARTIFACTORY_ACCESS_TOKEN = credentials('JFROG_ACCESS_TOKEN')
    }
    stages {
        stage('Build') { 
            steps {
                sh 'jf mvn-config' 
                sh 'jf rt build-add-git'
                sh 'jf rt build-add-dependencies'
                sh 'jf mvn -B clean install' 
            }
        }
        stage('Xray Scan'){
            steps {
                sh 'jf rt upload --url ${ART_URL} --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/marathon.war Marathon-App-Jenkins/'
           }
        }
        stage('JFrog Build Publish'){
            steps {
                sh 'jf rt upload "target/*" generic-local --build-name war-build --build-number 1'
                sh 'jf rt build-publish --url ${ART_URL} --access-token ${ARTIFACTORY_ACCESS_TOKEN} war-build 1'
           }
        }
    }
}
