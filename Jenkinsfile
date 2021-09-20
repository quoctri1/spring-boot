OCTOPUS_SPACE_NAME = 'Default'
OCTOPUS_PROJECT_NAME = 'dev-spring-boot'
OCTOPUS_CHANNEL_NAME = 'Default'
OCTOPUS_RELEASE_VERSION = '0.0.6'
OCTOPUS_PACKAGE_VERSION = ${env.BUILD_NUMBER}

pipeline {
    agent {
        label 'slave_node'
    }

    stages {
        stage('Example') {
            steps {
                echo 'Hello World'

                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
                }
            }
        }
    }
}
