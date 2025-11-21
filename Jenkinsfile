pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Source already checked out by Jenkins teeeheee"           }
        }

        stage('YAML Validation') {
            steps {
                sh '''
                echo "Validating YAML..."
                yamllint Policies || exit 1
                '''
            }
        }

        stage('Package Artifacts') {
            steps {
                sh '''
                rm -f artifact.zip
                zip -r artifact.zip Policies
                '''
            }
        }

        stage('KPS Update (Simulated)') {
            steps {
                sh '''
                echo "Updating KPS..."
                echo "KPS updated with new config" >> /var/log/jenkins-logs/kps-update.log
                '''
            }
        }

        stage('Import Backend API (Simulated)') {
            steps {
                sh '''
                mkdir -p /opt/axway/backend
                cp Policies/Payment/v1/Transfer.yaml /opt/axway/backend/
                echo "Backend API imported" >> /var/log/jenkins-logs/axway.log
                '''
            }
        }

        stage('Import Frontend API (Simulated)') {
            steps {
                sh '''
                mkdir -p /opt/axway/frontend
                cp Policies/Payment/v1/Transfer.yaml /opt/axway/frontend/
                echo "Frontend API published" >> /var/log/jenkins-logs/axway.log
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment pipeline completed successfully."
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
