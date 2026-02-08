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
                powershell script: "powershell -ExecutionPolicy Bypass -File scripts/install_chrome.ps1", returnStatus: true
            }
        }

        stage("Install ChromeDriver") {
            steps {
                powershell script: "powershell -ExecutionPolicy Bypass -File scripts/install_chromedriver.ps1", returnStatus: true
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
                // Поправи пътя до csproj според твоя репо
                bat 'dotnet test SeleniumIDE/SeleniumIde.csproj --logger "trx;LogFileName=test_results.trx"'
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
