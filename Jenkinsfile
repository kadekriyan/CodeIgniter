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
        stage('Install PHP & Composer') {
            steps {
                sh '''
                sudo apt-get update
                sudo apt-get install -y php-cli php-mbstring unzip curl
                curl -sS https://getcomposer.org/installer | php
                sudo mv composer.phar /usr/local/bin/composer
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
                sh 'vendor/bin/phpunit'
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
                // Tambahkan perintah deploy sesuai kebutuhan
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
