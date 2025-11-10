pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Java\\jdk-21.0.5'
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Clean & Build') {
            steps {
                echo "Building project with full debug logs..."
                bat './mvnw clean install -X'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests with debug output..."
                bat './mvnw test -X'
            }
        }

        stage('Check Code Quality') {
            steps {
                echo "Running code quality checks..."
                bat './mvnw verify -X'
            }
        }

        stage('Run App') {
            steps {
                echo "Starting Spring Boot app..."
                bat './mvnw spring-boot:run -Dspring-boot.run.profiles=dev'
            }
        }

        stage('Capture Logs') {
            steps {
                echo "Archiving logs for AI agents..."
                bat 'mkdir -p logs'
                bat  'cp -r target/**/*.log logs/'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'logs/**/*', allowEmptyArchive: true
            echo 'All logs archived'
        }
    }
}
