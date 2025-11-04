pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/Kennibravo/jenkins-laravel.git'
                bat 'composer install --ignore-platform-reqs'
                bat 'copy .env.example .env'
                bat 'php artisan key:generate'
            }
        }
        stage('Test') {
            steps {
                bat './vendor/bin/phpunit'
            }
        }
    }
}
