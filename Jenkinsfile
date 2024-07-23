pipeline {
    agent any

    tools {
        jdk 'JDK11'
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/IpshitaCT/Jenkins_Task.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Your SonarQube Server') {
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=your_project_key \
                            -Dsonar.sources=path/to/your/code \
                            -Dsonar.host.url=your_sonarqube_url \
                            -Dsonar.login=your_sonarqube_token_or_username_password
                    '''
                }
            }
        }
        stage('Setup Python Environment') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install lizard
                '''
            }
        }
        stage('Cyclomatic Complexity') {
            steps {
                sh '''
                    . venv/bin/activate
                    venv/bin/lizard path/to/your/code
                '''
            }
        }
    }

    post {
        success {
            mail to: 'ipshitachaudhary2000@gmail.com',
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' succeeded."
        }
        failure {
            mail to: 'ipshitachaudhary2000@gmail.com',
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed."
        }
    }
}
