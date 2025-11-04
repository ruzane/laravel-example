pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('LARAVEL_TOKEN')
        SONARQUBE_SCANNER = tool 'sonarqube'
        SONAR_ORG = 'zimswitchdev'
        SONAR_PROJECT = 'zimswitchdev_laravel-project'
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/Kennibravo/jenkins-laravel.git'
                bat '''
                    composer install --ignore-platform-reqs
                    copy /Y .env.example .env
                    php artisan key:generate
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    bat """
                        "${SONARQUBE_SCANNER}\\bin\\sonar-scanner.bat"^
                        -Dsonar.organization=${SONAR_ORG} ^
                        -Dsonar.projectKey=${SONAR_PROJECT} ^
                        -Dsonar.sources=. ^
                        -Dsonar.host.url=https://sonarcloud.io ^
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Test') {
            steps {
                bat './vendor/bin/phpunit'
            }
        }
    }
}

