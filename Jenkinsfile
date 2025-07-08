pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/AgengManggala/php-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'composer install'
            }
        }

        stage('Unit Test') {
            steps {
                sh './vendor/bin/phpunit tests'
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
