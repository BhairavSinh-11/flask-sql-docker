pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {

                dir('/app') {

                    sh 'git pull'

                    sh 'docker-compose down'

                    sh 'DOCKER_BUILDKIT=0 docker-compose build'

                    sh 'docker-compose up -d'

                }

            }
        }

    }
}