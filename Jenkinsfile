stage('Validate Config') {
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