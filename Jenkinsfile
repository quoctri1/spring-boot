pipeline {
    agent { 
      label 'slave_node'
    }

    stages {
        stage('Build package') {
            steps {
                sh './mvnw package'
                sh 'ls -la'
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