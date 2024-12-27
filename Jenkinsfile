pipeline {

    agent any

    environment {
        PYTHON_PATH = 'C:/Users/Sujit Chaugule/AppData/Local/Programs/Python/Python311;C:/Users/Sujit Chaugule/AppData/Local/Programs/Python/Python311/Scripts;'
        SONAR_SCANNER_PATH = 'D:/altered/sonar-scanner-6.2.1.4610-windows-x64/bin'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') { 
            steps {
                bat '''REM Update PATH to include the Scripts directory where pip is located
                    set PATH=C:\\Users\\Sujit Chaugule\\AppData\\Local\\Programs\\Python\\Python311\\Scripts;C:\\Users\\Sujit Chaugule\\AppData\\Local\\Programs\\Python\\Python311;%PATH% 
                                                     REM Check Python version     
                      python --version || (echo "Python not found. Exiting." && exit 1)                      
                                  REM Check pip version      
                                             pip --version || (echo "Pip not found. Exiting." && exit 1)  
                                                                             REM Example: Install a Python package 
                                                                                             pip install requests 
                                                                                                             '''
            }}

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')
            }
            steps {
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || (echo "SonarQube scanner not found. Exiting." && exit 1)
                sonar-scanner -Dsonar.projectKey=demoJenkins ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }

}
