pipeline {
    agent any

    tools {
        maven 'M3'
    }

    stages {
        stage('Checkout') {
            steps {
                // Get code from GitHub repository with explicit ref spec
                git branch: 'main',
                    url: 'https://github.com/LiamJQuinn/lbg-hello-world-maven.git',
                    changelog: false,
                    poll: false
            }
        }

        stage('Build') {
            steps {
                // Clean and compile the project
                sh "mvn clean compile"
            }
        }

        stage('Test') {
            steps {
                // Run tests and generate reports
                sh "mvn test"
            }
            post {
                always {
                    // Publish test results
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                // Package the application while skipping tests
                sh "mvn package -DskipTests"
            }
            post {
                success {
                    // Archive the JAR file
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            // Clean workspace after build
            cleanWs()
        }
    }
}
