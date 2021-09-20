OCTOPUS_SPACE_NAME='Default'
OCTOPUS_PROJECT_NAME='dev-spring-boot'
OCTOPUS_CHANNEL_NAME='Default'
OCTOPUS_RELEASE_VERSION='0.0.6'
OCTOPUS_PACKAGE_VERSION =${env.BUILD_NUMBER}

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
            }
            steps {
                sh (script: "curl -X GET http://localhost:8080/api/spaces \
                            -H \"X-Octopus-ApiKey: ${env.OCTOPUS_API_TOKEN}\"")
            }
        }
    }
}
