pipeline {
    agent any

    environment {
        DOTNET_VERSION = "6.0"
    }

    stages {
        stage("Checkout Code") {
            steps {
                checkout scm
            }
        }

        stage("Set up .NET Core") {
            steps {
                bat "dotnet --version"
            }
        }

        stage("Restore Dependencies") {
            steps {
                bat "dotnet restore"
            }
        }

        stage("Build Solution") {
            steps {
                bat "dotnet build"
            }
        }

        stage("Run Selenium Tests") {
            steps {
                // Стартираме тестовете и генерираме .trx файл в TestResults папката
                bat 'dotnet test SeleniumIDE/SeleniumIde.csproj --logger "trx;LogFileName=test_results.trx" --results-directory TestResults'
            }
        }
    }

    post {
        always {
            // Архивиране на .trx файловете
            archiveArtifacts artifacts: "TestResults/*.trx", fingerprint: true
            
            // Пращане на резултатите към JUnit step
            junit testResults: "TestResults/*.trx", allowEmptyResults: true
        }
    }
}

