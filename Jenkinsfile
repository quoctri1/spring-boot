
pipeline {
    agent {
        label 'slave_node'
    }
    environment {
        OCTOPUS_API_TOKEN = credentials('octopus_api_token')
        OCTOPUS_SPACE_NAME = 'Default'
        OCTOPUS_PROJECT_NAME = 'dev-spring-boot'
        OCTOPUS_CHANNEL_NAME = 'Default'
        OCTOPUS_PACKAGE_VERSION = "11"
        OCTOPUS_RELEASE_VERSION = "0.0.6"
        OCTOPUS_ENVIRONMENT = "dev"
    }

    stages {
        stage('Sonarqube check') {
            steps {
                echo 'Check sonarqube server'
            }
        }
        // stage('Build package') {
        //     steps {
        //         sh './mvnw package'
        //     }
        // }
        // stage('Build and push image') {
        //     environment {
        //         KANIKO_AUTHEN = credentials('docker')
        //     }
        //     steps {
        //         sh "docker run --rm --name kaniko -v ${env.WORKSPACE}:/workspace -v ${env.KANIKO_AUTHEN}:/kaniko/.docker/config.json gcr.io/kaniko-project/executor:latest --dockerfile=/workspace/Dockerfile --destination=phqtri/spring-boot:${OCTOPUS_PACKAGE_VERSION}"
        //     }
        // }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        // stage('Crete release') {
        //     steps {
        //         script {
        //             def spaceId,projectId,channelId
        //             //Spaces
        //             def spaces = sh(returnStdout: true, script: "curl -sX GET http://localhost:8080/api/spaces -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\"").trim()
        //             def releaseInfo = readJSON text: spaces
        //             for (int i = 0; i < releaseInfo.Items.size(); i++) {
        //                 if (releaseInfo.Items[i].Name == "${OCTOPUS_SPACE_NAME}") {
        //                     spaceId = "${releaseInfo.Items[i].Id}"
        //                 }
        //             }
        //             echo "spaceId: ${spaceId}"

        //             //Projects
        //             def projects = sh(returnStdout: true, script: "curl -sX GET http://localhost:8080/api/${spaceId}/projects/ -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\"").trim()
        //             def projectsInfo = readJSON text: projects
        //             for (int i = 0; i < projectsInfo.Items.size(); i++) {
        //                 if (projectsInfo.Items[i].Name == "${OCTOPUS_PROJECT_NAME}") {
        //                     projectId = "${projectsInfo.Items[i].Id}"
        //                 }
        //             }
        //             echo "projectId: ${projectId}"

        //             //Channels
        //             def channels = sh(returnStdout: true, script: "curl -sX GET http://localhost:8080/api/${spaceId}/projects/${projectId}/channels -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\"").trim()
        //             def channelsInfo = readJSON text: channels
        //             for (int i = 0; i < channelsInfo.Items.size(); i++) {
        //                 if (channelsInfo.Items[i].Name == "${OCTOPUS_CHANNEL_NAME}") {
        //                     channelId = "${channelsInfo.Items[i].Id}"
        //                 }
        //             }
        //             echo "channelId: ${channelId}"

        //             //Template
        //             def selectedPackages = []
        //             def templates = sh(returnStdout: true, script: "curl -sX GET http://localhost:8080/api/${spaceId}/deploymentprocesses/deploymentprocess-${projectId}/template?channel=${channelId} -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\"").trim()
        //             def templatesInfo = readJSON text: templates

        //             for (int i = 0; i < templatesInfo.Packages.size(); i++) {
        //                 selectedPackageJson = "{ \"ActionName\": \"${templatesInfo.Packages[i].ActionName}\", \"PackageReferenceName\": \"${templatesInfo.Packages[i].PackageReferenceName}\", \"Version\": \"${OCTOPUS_PACKAGE_VERSION}\"}"
        //                 selectedPackages[i] = selectedPackageJson
        //             }
        //             releaseJson = "{\"ChannelId\": \"${channelId}\", \"ProjectId\":  \"${projectId}\", \"Version\": \"${templatesInfo.NextVersionIncrement}\", \"SelectedPackages\": ${selectedPackages}}"
        //             OCTOPUS_RELEASE_VERSION = "${templatesInfo.NextVersionIncrement}"
        //             //Create release
        //             def release = sh(returnStdout: true, script: "curl -X POST http://localhost:8080/api/${spaceId}/releases -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\" -H \"Content-Type: application/json\" --data '${releaseJson}'").trim()
        //             echo "release: ${release}"
        //         }
        //     }
        // }
        stage('Start release') {
            steps {
                script {
                    def spaceId,environmentId,releaseId
                    //Spaces
                    def spaces = sh(returnStdout: true, script: "curl -sX GET http://localhost:8080/api/spaces -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\"").trim()
                    def spacesInfo = readJSON text: spaces
                    for (int i = 0; i < spacesInfo.Items.size(); i++) {
                        if (spacesInfo.Items[i].Name == "${OCTOPUS_SPACE_NAME}") {
                            spaceId = "${spacesInfo.Items[i].Id}"
                        }
                    }
                    echo "spaceId: ${spaceId}"

                    //Environment
                    def environments = sh(returnStdout: true, script: "curl -sX GET http://localhost:8080/api/${spaceId}/environments -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\"").trim()
                    def environmentsInfo = readJSON text: environments
                    for (int i = 0; i < environmentsInfo.Items.size(); i++) {
                        if (environmentsInfo.Items[i].Name == "${OCTOPUS_ENVIRONMENT}") {
                            environmentId = "${environmentsInfo.Items[i].Id}"
                        }
                    }
                    echo "environmentId: ${environmentId}"

                    //Releases
                    def releases = sh(returnStdout: true, script: "curl -sX GET http://localhost:8080/api/${spaceId}/releases -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\"").trim()
                    def releasesInfo = readJSON text: releases
                    for (int i = 0; i < releasesInfo.Items.size(); i++) {
                        if (releasesInfo.Items[i].Version == "${OCTOPUS_RELEASE_VERSION}") {
                            releaseId = "${releasesInfo.Items[i].Id}"
                        }
                    }
                    echo "releaseId: ${releaseId}"

                    //Deployment
                    deploymentJson = "{\"ReleaseId\": \"${releaseId}\", \"EnvironmentId\": \"${environmentId}\"}"
                    def deployment = sh(returnStdout: true, script: "curl -sX POST http://localhost:8080/api/${spaceId}/deployments -H \"X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}\" -H \"Content-Type: application/json\" --data '${deploymentJson}'").trim()
                    def deploymentsInfo = readJSON text: deployment
                    echo "deploymentsInfo: ${deploymentsInfo}"
                }
            }
        }
    }
}
