pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {

                withCredentials([file(credentialsId: 'jenkins-env-file', variable: 'ENV_FILE_PATH')]) {

                    sh '''
                        cp $ENV_FILE_PATH .env

                        docker-compose --env-file .env down

                        DOCKER_BUILDKIT=0 docker-compose --env-file .env build

                        docker-compose --env-file .env up -d
                    '''
                }
            }
        }

        stage('Initialize Database') {
            steps {

                withCredentials([file(credentialsId: 'jenkins-env-file', variable: 'ENV_FILE_PATH')]) {

                    sh '''
                        cp $ENV_FILE_PATH .env

                        set -a
                        . ./.env
                        set +a

                        sleep 15

                        docker exec -i mysql_db mysql \
                            -u root \
                            -p"${MY_SQL_ROOT_PASSWORD}" \
                            "${MY_SQL_DATABASE}" \
                            < schema.sql
                    '''
                }
            }
        }
    }
}