pipeline {
    agent any

    environment {
        ANSIBLE_INVENTORY = "/home/alaa/spring-petclinic/library/ansible-inventory"
        DEPLOY_SCRIPT_PATH = "/home/alaa/spring-petclinic/deploy_petclinic.yml"
    } 

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
                // Runs Maven build without running tests
                sh 'mvn clean package -DskipTests'
            }
        }
	
	stage('SonarQube Analysis') {
            steps {
        	withSonarQubeEnv('MySonarQubeServer') {
	            sh """
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=petclinic \
                    -Dsonar.projectName='Spring PetClinic' \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.token=sqp_fb22a2fd97ba88b4c15b3c47f7a9a353b94019f2
		    """
        	}
    	    }
        }

        stage('Test') {
            steps {
                // Runs the test phase separately to capture test results
                sh 'mvn test'
            }
        }
	
        stage('Deploy) {
            steps {
                script {
                    ansiblePlaybook(
                        playbook: "${DEPLOY_SCRIPT_PATH}",
                        inventory: "${ANSIBLE_INVENTORY}",
                        credentialsId: 'ssh-credentials-id'
                    )
                }
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

