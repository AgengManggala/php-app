pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh '''
                    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
                    php composer-setup.php
                    php -r "unlink('composer-setup.php');"
                    php composer.phar install
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh 'php composer.phar run-script test || php vendor/bin/phpunit tests'
            }
            post {
                success {
                    echo 'Tes berhasil!'
                }
                failure {
                    echo 'Tes gagal!'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker build -t php-app .'
                sh 'docker rm -f php-app-container || true'
                sh 'docker run -d --name php-app-container -p 9000:80 php-app'
            }
        }
    }
}
