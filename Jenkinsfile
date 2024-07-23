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
        stage('SonarCloud Scan') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('SonarCloud') {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=IpshitaCT_Jenkins_Task\
                            -Dsonar.organization=ipshitact \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=a2c51ef132f9abc7dce60942dc62050418e4040f"
                    }
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
