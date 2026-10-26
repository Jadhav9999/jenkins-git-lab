
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is checked out by Jenkins SCM'
            }
        }

        stage('Build') {
            steps {
                bat '''
                    @echo off
                    echo Starting application build...
                    if not exist app.txt exit /b 1
                    echo Build validation successful
                '''
            }
        }

        stage('Test') {
            steps {
                bat '''
                    @echo off
                    findstr /C:"application code from git" app.txt
                    if errorlevel 1 exit /b 1
                    echo Application test passed
                '''
            }
        }
    }

    post {
        success {
            echo 'PIPELINE SUCCESS'
        }
        failure {
            echo 'PIPELINE FAILED - Check the failed stage'
        }
    }
}
