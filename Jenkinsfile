pipeline {
    agent any
    
    options {
        // Define options for the pipeline
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    parameters {
        // Define parameters for the pipeline
        string(name: 'GIT_URL', defaultValue: 'https://github.com/your/repository.git', description: 'Git repository URL')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Git branch to build')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests during the build')
        choice(name: 'DEPLOY_ENV', choices: ['staging', 'production'], description: 'Select deployment environment')
    }

    environment {
        // Define environment variables if needed
        // Example: MY_VARIABLE = 'some_value'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Use the GIT_URL and BRANCH parameters
                    git branch: "${params.BRANCH}", url: "${params.GIT_URL}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build your project
                    // Example: Maven build
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                script {
                    // Run tests if the RUN_TESTS parameter is true
                    // Example: JUnit tests
                    sh 'mvn test'
                }
            }
        }

        stage('Deploy') {
            when {
                expression { params.DEPLOY_ENV == 'production' }
            }
            steps {
                script {
                    // Deploy your application to the selected environment
                    // Example: Copy artifacts to a server
                    sh 'rsync -avz target/ user@your-server:/path/to/production'
                }
            }
        }

        stage('Notify') {
            steps {
                script {
                    // Notify about the build status
                    // Example: Send email or Slack notification
                    echo 'Build completed successfully!'
                }
            }
        }
    }

    post {
        success {
            script {
                // Actions to be taken on successful build
                echo 'Build successful!'
            }
        }

        failure {
            script {
                // Actions to be taken on build failure
                echo 'Build failed. Please check the logs for more details.'
            }
        }

        always {
            script {
                // Actions to be taken regardless of build status
                echo 'Pipeline completed.'
            }
        }
    }
}
