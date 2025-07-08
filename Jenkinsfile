pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/AgengManggala/php-app.git'
            }
        }

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
                sh 'php composer.phar install' // just in case
                sh 'php composer.phar dump-autoload'
                sh 'php composer.phar run-script test || ./vendor/bin/phpunit tests'
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
                sh 'docker run -d --name php-app-container -p 9000:80 php-app'
            }
        }
    }
}
