pipeline {
    agent any

    environment {
        CHROME_VERSION = "144.0.7559.133"
        CHROMEDRIVER_VERSION = "144.0.7559.133"
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

        stage("Skip Chrome Uninstall") {
            steps {
                echo "Skipping Chrome uninstall on Jenkins"
            }
        }

        stage("Install Chrome") {
            steps {
                // Поправи пътя спрямо твоя workspace
                powershell script: "powershell -ExecutionPolicy Bypass -File Selenium/scripts/install_chrome.ps1", returnStatus: true
            }
        }

        stage("Install ChromeDriver") {
            steps {
                powershell script: "powershell -ExecutionPolicy Bypass -File Selenium/scripts/install_chromedriver.ps1", returnStatus: true
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
                // Поправи пътищата спрямо действителните csproj файлове
                bat 'dotnet test SeleniumIDE.Tests/SeleniumIDE.Tests.csproj --logger "trx;LogFileName=test_results1.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: "**/*.trx", fingerprint: true
            junit "**/*.trx"
        }
    }
}
