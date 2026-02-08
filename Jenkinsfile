pipeline {
    agent any

    environment {
        CHROME_VERSION = "144.0.7559.133"
        CHROMEDRIVER_VERSION = "144.0.7559.133"
        CHROME_INSTALL_PATH = "C:/Program Files/Google/Chrome/Application"
        CHROMEDRIVER_PATH = "C:/Program Files/Google/Chrome/Application"
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

        stage("Uninstall Current Chrome") {
    steps {
        bat 'echo Uninstalling current Chrome...'
        bat 'winget uninstall --id Google.Chrome -e --silent || echo "Skipping Chrome uninstall"'
    }

    continueOnError true
}

        stage("Install Specific Chrome Version") {
            steps {
                bat "powershell -ExecutionPolicy Bypass -File install_chrome.ps1"
            }
        }

        stage("Download and Install ChromeDriver") {
            steps {
                bat "powershell -ExecutionPolicy Bypass -File install_chromedriver.ps1"
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
                bat 'dotnet test SeleniumProject1/SeleniumProject1.csproj --logger "trx;LogFileName=test_results1.trx"'
                bat 'dotnet test SeleniumProject2/SeleniumProject2.csproj --logger "trx;LogFileName=test_results2.trx"'
                bat 'dotnet test SeleniumProject3/SeleniumProject3.csproj --logger "trx;LogFileName=test_results3.trx"'
                bat 'dotnet test SeleniumProject4/SeleniumProject4.csproj --logger "trx;LogFileName=test_results4.trx"'
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
