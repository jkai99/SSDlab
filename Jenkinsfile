pipeline {
    agent any
    environment {
        PATH = "C:\\Program Files\\php-8.3.9;${env.PATH}"
        PHPRC = "C:\\Program Files\\php-8.3.9"
    }
    stages {
        stage('Verify PHP and OpenSSL') {
            steps {
                sh 'php --version'
                sh 'php -r "print_r(openssl_get_cert_locations());"'
            }
        }
        stage('Setup') {
            steps {
                // Install Composer
                sh '''
                    EXPECTED_CHECKSUM="$(php -r 'copy("https://composer.github.io/installer.sig", "php://stdout");')"
                    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
                    ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")"
                    if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]; then
                        >&2 echo 'ERROR: Invalid installer checksum'
                        rm composer-setup.php
                        exit 1
                    fi
                    php composer-setup.php --install-dir=/usr/local/bin --filename=composer
                    RESULT=$?
                    rm composer-setup.php
                    exit $RESULT
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'composer install'
            }
        }
        stage('Test') {
            steps {
                sh './vendor/bin/phpunit tests'
            }
        }
    }
}
