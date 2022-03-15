pipeline {
    agent any
    stages {
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "jfrogeval",
                    url: https://evaluate.jfrog.io,
                    credentialsId: CREDENTIALS
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "jfrogeval",
                    releaseRepo: "myrepo",
                    snapshotRepo: "mylocalrepo"
                )

                //rtMavenResolver (
                 //   id: "MAVEN_RESOLVER",
                 //   serverId: "jfrogeval",
                 //   releaseRepo: ARTIFACTORY_VIRTUAL_RELEASE_REPO,
                 //   snapshotRepo: ARTIFACTORY_VIRTUAL_SNAPSHOT_REPO
                //)
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: MAVEN_TOOL, // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean build',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jfrogeval"
                )
            }
        }
    }
}
