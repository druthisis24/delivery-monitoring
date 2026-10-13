
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Project Files') {
            steps {
                sh '''
                    test -f delivery_metrics.py
                    test -f requirements.txt
                    test -f prometheus.yml
                    test -f alert_rules.yml
                    test -f Dockerfile
                    test -f docker-compose.yml
                    echo "All required project files are present."
                '''
            }
        }

        stage('Validate Dockerfile') {
            steps {
                sh '''
                    grep -q "FROM python:" Dockerfile
                    echo "Dockerfile validation passed."
                '''
            }
        }
    }

    post {
        success {
            echo 'Delivery monitoring pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
