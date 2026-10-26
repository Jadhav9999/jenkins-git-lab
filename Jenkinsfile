
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
                    echo Testing test.txt...

                    if not exist test.txt (
                        echo ERROR: test.txt not found
                        exit /b 1
                    )

                    findstr /C:"application code from git" test.txt

                    if errorlevel 1 (

                        echo ERROR: test.txt validation failed
                        exit /b 1
                    )

                    echo SUCCESS: test.txt validation passed
                '''
            }
        }

        stage('Validate Config') 

{
        stage('Deploy to TEST') {
            steps {
                bat '''
                    @echo off
                    echo Starting TEST deployment...

                    if not exist deployment\\test mkdir deployment\\test

                    copy /Y app.txt deployment\\test\\app.txt

                    if errorlevel 1 exit /b 1

                    echo TEST deployment completed successfully
                '''
            }
        }

            steps {
                bat '''
                    @echo off
                    echo Checking application configuration...

                    if not exist config.txt (
                        echo ERROR: config.txt not found
                        exit /b 1
                    )

                    findstr /X /C:"ENVIRONMENT=TEST" config.txt

                    if errorlevel 1 (
                        echo ERROR: Incorrect environment configuration
                        exit /b 1
                    )

                    echo SUCCESS: Configuration validation passed
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


