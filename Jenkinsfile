pipeline {
    agent any

    stages {
        // Этап 1: Забираем код из GitHub
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // Этап 2: Инициализация Go-модуля (если нужно)
        stage('Init Go Module') {
            steps {
                script {
                    // Проверяем, есть ли go.mod
                    def hasGoMod = fileExists('go.mod')
                    if (!hasGoMod) {
                        sh 'go mod init github.com/your-username/your-repo'
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

        // Этап 4: Запуск тестов
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
