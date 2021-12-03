pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               sh 'composer install'
               // npm install
               // and so on to compile your assets.
            }
        }
        stage('Test') {
            steps {
               sh './vendor/bin/phpunit'
            }
        }

        stage('Deploy to staging') {
            steps {
               sh 'ssh -o StrictHostkeyChecking=no forum-deploy@192.168.56.105 "cd forum; \
                  git pull origin master; \
                  composer install; \
                  php artisan migrate --force; \
                  php artisan cache:clear; \
                  php artisan config:cache "'
            }
        }

        stage('Deploy to production') {
            input {
                message "Shall we deploy to production?"
                ok "Yes Please"
            }
            steps {
               sh 'ssh -o StrictHostkeyChecking=no forum-deploy@192.168.56.101 "cd forum; \
                  git pull origin master; \
                  composer install --optimize-autoloader --no-dev; \
                  php artisan migrate --force; \
                  php artisan cache:clear; \
                  php artisan config:cache "'
            }
        }
    }
}
