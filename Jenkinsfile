pipeline {
    agent any

    stages {
        stage('Checkout & Setup') {
            steps {
                // 1. Checkout the code to the default workspace (e.g., /var/jenkins_home/workspace/flask-sql-docker)
                checkout scm
                
                // 2. Create the /app directory if it doesn't exist
                sh 'mkdir -p /app'

                // 3. Copy the checked-out files to /app
                // Note: Ensure your repo has the necessary files (Dockerfile, docker-compose.yml, .env, schema.sql)
                sh 'cp -r ./* /app/'
                
                // Alternatively, if you want to keep everything in the workspace and just change the working directory:
                // dir('path/to/your/workspace') { ... }
                
                // Now switch to /app and ensure Git is safe
                dir('/app') {
                    sh 'git config --global --add safe.directory /app'
                    
                    // 4. Pull latest changes (Now this works because /app is a git repo)
                    sh 'git pull origin main'
                }
            }
        }

        stage('Deploy') {
            steps {
                dir('/app') {
                    sh 'docker-compose --env-file .env down'
                    sh 'DOCKER_BUILDKIT=0 docker-compose --env-file .env build'
                    sh 'docker-compose --env-file .env up -d'
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
}