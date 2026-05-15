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
                dir('/app') {
                    sh '''
                        cp /var/jenkins_home/workspace/flask_CICD/.env .env 2>/dev/null || cp $ENV_FILE_PATH .env

                        
                        set -a
                        source .env
                        set +a

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