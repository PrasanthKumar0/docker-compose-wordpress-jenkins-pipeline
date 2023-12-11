pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/PrasanthKumar0/docker-compose-wordpress-jenkins-pipeline.git']]])
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying WordPress...'
                sh 'docker-compose up -d'
            }
        }

        stage('Show IP Address') {
            steps {
                script {
                    // Get the container ID of the running WordPress container
                    def containerId = sh(script: 'docker ps -q --filter "name=wordpress"', returnStdout: true).trim()

                    // Get the IP address of the container
                    def ipAddress = sh(script: 'docker inspect -f \'{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}\' ${containerId}', returnStdout: true).trim()

                    echo "WordPress is accessible at http://${ipAddress}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            // Cleanup Docker containers
            sh 'docker-compose down'
        }
    }
}
