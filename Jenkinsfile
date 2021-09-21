
pipeline {
    agent {
        label 'slave_node'
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
        //         sh "docker run --rm --name kaniko -v ${env.WORKSPACE}:/workspace -v ${env.KANIKO_AUTHEN}:/kaniko/.docker/config.json gcr.io/kaniko-project/executor:latest --dockerfile=/workspace/Dockerfile --destination=phqtri/spring-boot:${env.BUILD_NUMBER}"
        //     }
        // }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Crete release') {
            environment {
                OCTOPUS_API_TOKEN = credentials('octopus_api_token')
                OCTOPUS_SPACE_NAME = 'Default'
                OCTOPUS_PROJECT_NAME = 'dev-spring-boot'
                OCTOPUS_CHANNEL_NAME = 'Default'
                OCTOPUS_RELEASE_VERSION = '0.0.6'
                OCTOPUS_PACKAGE_VERSION = "2"
            }
            steps {
                script {
                    def spaceId,projectId,channelId
                    //Spaces
                    def spaces = sh(returnStdout: true, script: 'curl -sX GET http://localhost:8080/api/spaces -H "X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}"').trim()
                    def releaseInfo = readJSON text: spaces
                    for (int i = 0; i < releaseInfo.Items.size(); i++) {
                        if (releaseInfo.Items[i].Name == "${env.OCTOPUS_SPACE_NAME}") {
                            spaceId = "${releaseInfo.Items[i].Id}"
                        }
                    }
                    echo "spaceId: ${spaceId}"
                    
                    //Projects
                    def projects = sh(returnStdout: true, script: 'curl -sX GET http://localhost:8080/api/${spaceId}/projects/ -H "X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}"').trim()
                    def projectsInfo = readJSON text: projects
                    for (int i = 0; i < projectsInfo.Items.size(); i++) {
                        if (projectsInfo.Items[i].Name == "${env.OCTOPUS_PROJECT_NAME}") {
                            projectId = "${projectsInfo.Items[i].Id}"
                        }
                    }
                    echo "projectId: ${projectId}"

                    //Channels
                    def channels = sh(returnStdout: true, script: 'curl -sX GET http://localhost:8080/api/${spaceId}/projects/${projectId}/channels -H "X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}"').trim()
                    echo "channels: ${channels}"
                    def channelsInfo = readJSON text: channels
                    // for (int i = 0; i < channelsInfo.Items.size(); i++) {
                    //     if (channelsInfo.Items[i].Name == "${env.OCTOPUS_CHANNEL_NAME}") {
                    //         channelId = "${channelsInfo.Items[i].Id}"
                    //     }
                    // }
                    // echo "channelId: ${channelId}"

                    //Template
                    // def selectedPackages = []
                    // def templates = sh(returnStdout: true, script: 'curl -sX GET http://localhost:8080/api/${spaceId}/deploymentprocesses/deploymentprocess-${projectId}/template?channel=${channelId} -H "X-Octopus-ApiKey: ${OCTOPUS_API_TOKEN}"').trim()
                    // def templatesInfo = readJSON text: templates
                    // for (int i = 0; i < templatesInfo.Packages.size(); i++) {
                    //     selectedPackageJson = "{ \"ActionName\": \"${templatesInfo.Packages[i].ActionName}\", \"PackageReferenceName\": \"${templatesInfo.Packages[i].PackageReferenceName}\", \"Version\": \"${env.OCTOPUS_PACKAGE_VERSION}\"}"
                    //     selectedPackages[i] = selectedPackageJson
                    // }
                    // releaseJson = "{\"ChannelId\": \"${channelId}\", \"ProjectId\":  \"${projectId}\", \"Version\": \"${env.OCTOPUS_RELEASE_VERSION}\", \"SelectedPackages\": ${selectedPackages}}"
                    
                    //Create release
                    // def release = sh(returnStdout: true, script: 'curl -X POST http://localhost:8080/api/${spaceId}/releases -H "X-Octopus-ApiKey: ${env.OCTOPUS_API_TOKEN}" -H "Content-Type: application/json" --data \'${releaseJson}\'').trim()
                    // echo "release: ${release}"
                }
            }
        }
    }
}
