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
        stage('Build package') {
            steps {
                sh './mvnw package'
            }
        }
        stage('Build and push image') {
            environment {
                KANIKO_AUTHEN = credentials('docker')
            }
            steps {
                sh 'docker run --rm --name kaniko -v ${env.WORKSPACE}:/workspace -v $KANIKO_AUTHEN:/kaniko/.docker/config.json gcr.io/kaniko-project/executor:latest --dockerfile=/workspace/Dockerfile --destination=phqtri/spring-boot:${env.BUILD_NUMBER}'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
