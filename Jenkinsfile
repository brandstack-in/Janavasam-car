pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git Branch to Deploy')
        choice(name: 'ENVIRONMENT', choices: ['DEV', 'TEST', 'PROD'], description: 'Select Environment')
        string(name: 'DEPLOY_PATH', defaultValue: '/var/www/html', description: 'Deployment Path')
        booleanParam(name: 'RESTART_APACHE', defaultValue: true, description: 'Restart Apache After Deploy?')
    }

    environment {
        GIT_REPO = "https://github.com/brandstack-in/Janavasam-car.git"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: "${params.BRANCH_NAME}", url: "${GIT_REPO}"
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    echo "Deploying to ${params.ENVIRONMENT}"
                    cp -r public_html/* ${params.DEPLOY_PATH}/
                """
            }
        }

        stage('Restart Apache') {
            when {
                expression { params.RESTART_APACHE }
            }
            steps {
                sh "systemctl restart apache2"
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}
