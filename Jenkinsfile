pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Run Maven build without running tests
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                // Runs the test phase separately to capture test results
                sh 'mvn test'
            }
        }
    }

    post {
        always {
            // Actions to perform after the pipeline run completes, regardless of the result
            echo 'The pipeline has completed!'
        }

        success {
            // Actions to perform if the pipeline run is successful
            echo 'Build and test stages succeeded.'
        }

        failure {
            // Actions to perform if the pipeline run fails
            echo 'Build or test stages failed.'
        }
    }
}

