pipeline{

    agent any

    stages{

stage("Checkout"){
            steps{
                checkout scm
            }
        }

stage("Setup .NET"){
            steps{
                bat "dotnet --version"
            }
        }

        stage("Uninstall Chrome") {
            steps {
                bat '''
                echo Uninstalling Chrome...
                winget uninstall --id Google.Chrome -e --silent
                '''
            }
        }

        stage("Install Specific Chrome Version") {
            steps {
                bat '''
                echo Installing specific Chrome version...
                curl -L -o chrome_installer.exe https://dl.google.com/tag/s/appguid%3D%7B8A69D345-D564-463C-AFF1-A69D9E530F96%7D%26iid%3D%7B00000000-0000-0000-0000-000000000000%7D/googlechromestandaloneenterprise64.msi
                msiexec /i chrome_installer.exe /qn
                '''
            }
        }

        stage("Download ChromeDriver") {
            steps {
                bat '''
                echo Downloading ChromeDriver...
                curl -L -o chromedriver.zip https://storage.googleapis.com/chrome-for-testing-public/144.0.7559.133/win64/chromedriver-win64.zip
                tar -xf chromedriver.zip
                '''
            }
        }

        stage("Restore"){
            steps{
                bat "dotnet restore"
            }
        }
    
        stage("Build"){
            steps{
                bat "dotnet build"
            }
        }
    
        stage("Test"){
            steps{
                bat "dotnet test"
            }
        }
}
}