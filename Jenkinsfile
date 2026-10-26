
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Code checked out from GitHub'
            }
        }

        stage('Build') {
            steps {
                bat '''
                    @echo off
                    echo Starting build...
                    if not exist app.txt exit /b 1
                    echo Build successful
                '''
            }
        }

        stage('Test') {
            steps {
                bat '''
                    @echo off
                    echo Testing test.txt...

                    if not exist test.txt exit /b 1
                    findstr /C:"application code from git" test.txt
                    if errorlevel 1 exit /b 1

                    echo Test successful
                '''
            }
        }

        stage('Validate Config') {
            steps {
                bat '''
                    @echo off
                    echo Checking config.txt...

                    if not exist config.txt exit /b 1
                    findstr /X /C:"ENVIRONMENT=TEST" config.txt
                    if errorlevel 1 exit /b 1

                    echo Configuration validation successful
                '''
            }
        }

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

        stage('Approve PROD Deployment') {
            steps {
                input message: 'Build, tests, config validation and TEST deployment passed. Approve PROD?',
                      ok: 'Approve PROD'
            }
        }

        stage('Deploy to PROD') {
            steps {
                bat '''
                    @echo off
                    echo Starting PROD deployment...

                    if not exist deployment\\prod mkdir deployment\\prod
                    copy /Y app.txt deployment\\prod\\app.txt
                    if errorlevel 1 exit /b 1

                    echo PROD deployment completed successfully
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
