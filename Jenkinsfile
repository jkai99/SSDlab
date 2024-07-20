pipeline {
    agent any
    environment {
        PATH = "C:\\Program Files\\php-8.3.9;C:\\ProgramData\\Composer;${env.PATH}"
        PHPRC = "C:\\Program Files\\php-8.3.9"
    }
    stages {
        stage('Verify PHP and OpenSSL') {
            steps {
                sh 'php -c "C:\\Program Files\\php-8.3.9\\php.ini" --version'
                sh 'php -c "C:\\Program Files\\php-8.3.9\\php.ini" -m | findstr openssl'
            }
        }
        stage('Setup') {
            steps {
                // Ensure Composer is installed and accessible
                sh 'php -c "C:\\Program Files\\php-8.3.9\\php.ini" "C:\\ProgramData\\Composer\\composer.phar" --version'
            }
        }
        stage('Build') {
            steps {
                sh 'php -c "C:\\Program Files\\php-8.3.9\\php.ini" "C:\\ProgramData\\Composer\\composer.phar" install'
            }
        }
        stage('Test') {
            steps {
                sh 'php -c "C:\\Program Files\\php-8.3.9\\php.ini" ./vendor/bin/phpunit --log-junit logs/unitreport.xml -c tests/phpunit.xml tests'
            }
        }
    }
    post {
        always {
            junit 'logs/unitreport.xml'
        }
    }
}
