pipeline {
    agent any  // или docker 'golang:latest'

    stages {
        // Этап 1: Забираем код из GitHub
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],  // или ваша ветка
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/egorikk58/Jenkins_pipeline_test.git',
                        credentialsId: 'github-creds'  // ID из настроек Jenkins
                    ]]
                ])
            }
        }

        // Этап 2: Устанавливаем зависимости Go
        stage('Install Dependencies') {
            steps {
                sh 'go mod download'
            }
        }

        // Этап 3: Запускаем тесты (если есть)
        stage('Test') {
            steps {
                sh 'go test ./...'
            }
        }

        // Этап 4: Собираем бинарник
        stage('Build') {
            steps {
                sh 'go build -o myapp'  // создаёт бинарник 'myapp'
                archiveArtifacts artifacts: 'myapp', fingerprint: true  // сохраняем артефакт
            }
        }
    }

    // Пост-обработка (уведомления, очистка)
    post {
        always {
            cleanWs()  // очистка workspace
        }
        success {
            echo '✅ Сборка успешна!'
        }
        failure {
            echo '❌ Сборка упала!'
        }
    }
}
