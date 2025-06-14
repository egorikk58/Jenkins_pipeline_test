pipeline {
    agent any

    stages {
        // Этап 1: Забираем код из GitHub
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // Этап 2: Инициализация Go-модуля
        stage('Init Go Module') {
            steps {
                script {
                    if (!fileExists('go.mod')) {
                        // Используем правильный формат имени модуля
                        sh 'go mod init github.com/egorikk58/Jenkins_pipeline_test'
                    }
                }
            }
        }

        // Этап 3: Установка зависимостей
        stage('Install Dependencies') {
            steps {
                sh 'go mod tidy'
            }
        }

        // Этап 4: Запуск тестов (если есть)
        stage('Test') {
            steps {
                sh 'go test ./...'
            }
        }

        // Этап 5: Сборка
        stage('Build') {
            steps {
                sh 'go build -o myapp'
                archiveArtifacts artifacts: 'myapp', fingerprint: true
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo '✅ Сборка успешна!'
        }
        failure {
            echo '❌ Сборка упала!'
        }
    }
}
