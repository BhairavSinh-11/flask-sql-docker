pipeline {
    agent any

    stages {
        stage('Deploy') {
            steps {
                // Bind the secret file from Jenkins credentials
                withCredentials([file(credentialsId: 'jenkins-env-file', variable: 'ENV_FILE_PATH')]) {
                    sh '''
                        # Copy the secret file to .env in the current directory
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
                dir('/app') {
                    sh '''

                        export $(cat .env | xargs)
                        
                        sleep 20

                        docker exec -i mysql_db mysql \
                            -u root \
                            -p${MY_SQL_ROOT_PASSWORD} \
                            ${MY_SQL_DATABASE} \
                            < schema.sql
                    '''
                }
            }
        }
    }
}