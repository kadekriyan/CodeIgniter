pipeline {
    agent any
    environment {
        CI_ENV = 'production'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kadekriyan/CodeIgniter.git'
            }
        }
	stage('Install PHP') {
            steps {
                sh '''
                apt-get update
                apt-get install -y php-cli php-mbstring unzip curl
                '''
            }
        }
	stage('Install Composer') {
            steps {
                sh '''
                curl -sS https://getcomposer.org/installer | php
                mv composer.phar /usr/local/bin/composer
                '''
            }
        }
        stage('Install Dependencies') {
            steps {
               sh 'composer install --no-dev --optimize-autoloader'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'phpunit'
            }
            post {
                success {
                    junit 'application/tests/results/*.xml'
                }
                failure {
                    echo 'Tests failed!'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to production environment...'
                // Tambahkan perintah deploy sesuai kebutuhan Anda
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
