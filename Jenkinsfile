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
        stage('Install Dependencies') {
            steps {
                sh 'composer install --optimize-autoloader --ignore-platform-req=ext-dom --no-scripts'
            }
        }
        stage('Run Tests') {
            steps {
                script {
                    if (!fileExists('phpunit.xml')) {
                        writeFile file: 'phpunit.xml', text: '''<?xml version="1.0" encoding="UTF-8"?>
<phpunit bootstrap="vendor/autoload.php" colors="true">
    <testsuites>
        <testsuite name="CodeIgniter Test Suite">
            <directory suffix="Test.php">./tests</directory>
        </testsuite>
    </testsuites>
</phpunit>'''
                    }
                    
                    sh 'if [ -d "tests" ] || [ -d "application/tests" ]; then vendor/bin/phpunit; else echo "No tests found. Skipping tests."; fi'
                }
            }
            post {
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
