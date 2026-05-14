#jenkinsfile pipeline 

pipeline {
    agent any

    stages {

        stage('Pull Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/BhairavSinh-11/flask-sql-docker.git'
            }
        }

        stage('Stop Old Containers') {
            steps {
                sh 'docker-compose down'
            }
        }

        stage('Build Containers') {
            steps {
                sh 'DOCKER_BUILDKIT=0 docker-compose build'
            }
        }

        stage('Start Containers') {
            steps {
                sh 'docker-compose up -d'
            }
        }

    }
}