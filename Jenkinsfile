def rtServer, buildInfo

pipeline {
    agent {
        any {
        }
    }
    //parameters {
    //    string (name: 'ART_URL', defaultValue: 'https://evaluate.jfrog.io/artifactory', description: 'URL for JFrog Artifactory')
    //}
    environment {
        CI = true
        ARTIFACTORY_ACCESS_TOKEN = credentials('JFROG_ACCESS_TOKEN')
        ART_URL = "https://evaluate.jfrog.io/artifactory"
        BUILD_NAME = "war-build"
        BUILD_ID = 2
    }
    stages {
        stage('Build') { 
            steps {
                sh 'jf mvn-config' 
                sh 'jf rt build-add-git $BUILD_NAME $BUILD_ID'
                sh 'jf rt build-add-dependencies $BUILD_NAME $BUILD_ID "./integration"'
                sh 'jf mvn -B clean install' 
            }
        }
        stage('Xray Scan'){
            steps {
                sh 'jf rt upload --url ${ART_URL} --access-token ${ARTIFACTORY_ACCESS_TOKEN} --build-name $BUILD_NAME --build-number BUILD_ID target/marathon.war Marathon-App-Jenkins/'
           }
        }
        stage('JFrog Build Publish'){
            steps {
                sh 'jf rt build-publish --url $ART_URL --access-token $ARTIFACTORY_ACCESS_TOKEN $BUILD_NAME $BUILD_ID'
           }
        }
    }
}
