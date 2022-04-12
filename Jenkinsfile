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
        BUILD_NAME = "war-build-new"
        BUILD_ID = 10
    }
    stages {
        stage('Build') { 
            steps {
                sh 'jf mvn-config' 
                sh 'jf rt build-add-git $BUILD_NAME $BUILD_ID'
                sh 'jf rt build-add-dependencies $BUILD_NAME $BUILD_ID "integration/**/*.jar"'
                sh 'jf mvn -B clean install' 
            }
        }
        stage('Xray Scan'){
            steps {
                sh 'jf rt upload target/marathon.war Marathon-App-Jenkins --url ${ART_URL} --access-token ${ARTIFACTORY_ACCESS_TOKEN} --build-name $BUILD_NAME --build-number $BUILD_ID --project "x02"'
           }
        }
        stage('JFrog Build Publish'){
            steps {
                sh 'jf rt build-publish $BUILD_NAME $BUILD_ID --url $ART_URL --access-token $ARTIFACTORY_ACCESS_TOKEN  --project "x02"'
           }
        }
    }
}
