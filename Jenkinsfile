pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk' // adjust to your system
        PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Clean & Build') {
            steps {
                echo "Building project with full debug logs..."
                sh './mvnw clean install -X'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests with debug output..."
                sh './mvnw test -X'
            }
        }

        stage('Check Code Quality') {
            steps {
                echo "Running code quality checks..."
                sh './mvnw verify -X'
            }
        }

        stage('Run App') {
            steps {
                echo "Starting Spring Boot app..."
                sh './mvnw spring-boot:run -Dspring-boot.run.profiles=dev'
            }
        }

        stage('Capture Logs') {
            steps {
                echo "Archiving logs for AI agents..."
                sh 'mkdir -p logs'
                sh 'cp -r target/**/*.log logs/'  // capture Maven/Spring logs
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
