pipeline {
    agent any

    stages {

        stage('Deploy') {

            steps {

               sh '''
                docker-compose --env-file .env down

                DOCKER_BUILDKIT=0 docker-compose --env-file .env build

                docker-compose --env-file .env up -d
                '''
            }
        }

    }
        stage('Initialize Database') {
            steps {
                dir('/app') {
                    sh '''
                    export $(cat .env | xargs)
                    
                    sleep 20

                    docker exec -i mysql_db mysql \
                    -u root \
                    -p$MY_SQL_ROOT_PASSWORD \
                    $MY_SQL_DATABASE \
                    < schema.sql
                    '''
                }
            }
        }
    }