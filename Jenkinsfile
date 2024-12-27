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
                bat '''
                set PATH=%PYTHON_PATH%;%PATH%
                python --version || (echo "Python not found. Exiting." && exit 1)
                pip --version || (echo "Pip not found. Exiting." && exit 1)
                pip install -r requirements.txt || (echo "Pip install failed. Exiting." && exit 1)
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('Sonarqube-token')
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
