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
                sh 'docker run --rm -v $PWD:/app -w /app php:8.1-cli bash -c "apt update && apt install unzip -y && curl -sS https://getcomposer.org/installer | php && php composer.phar install"'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'docker run --rm -v $PWD:/app -w /app php:8.1-cli php composer.phar run-script test || php composer.phar vendor/bin/phpunit tests'
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
