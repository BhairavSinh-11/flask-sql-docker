pipeline {
    agent any

    stages {
        stage('Deploy') {
            steps {
                withCredentials([file(credentialsId: 'jenkins-env-file', variable: 'ENV_FILE_PATH')]) {
                    sh '''
                        # Copy secret file to .env

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
                
                    sh '''
                        export $(cat .env | xargs)

                        sleep 20

                        docker exec -i mysql_db mysql \
                            -u root \
                            -p${MYSQL_ROOT_PASSWORD} \
                            ${MYSQL_DATABASE} < schema.sql
                    '''
                
            }
        }
    }
}