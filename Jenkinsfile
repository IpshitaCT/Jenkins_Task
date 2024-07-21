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

        #stage('SonarQube Analysis') {
         #   steps {
          #      withSonarQubeEnv('SonarQube') {
           #         sh 'mvn sonar:sonar'
            #    }
            #}
        #}

        stage('Code Coverage') {
            steps {
                sh 'mvn clean verify'
                publishHTML(target: [
                    reportName: 'JaCoCo Report',
                    reportDir: 'target/site/jacoco',
                    reportFiles: 'index.html'
                ])
            }
        }

        stage('Cyclomatic Complexity') {
            steps {
                sh 'lizard .'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/lizard-report.xml', allowEmptyArchive: true
                }
            }
        }

        stage('Security Scan') {
            steps {
                sh 'dependency-check --project your-project --out .'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/dependency-check-report.html', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        success {
            mail to: 'your-email@example.com',
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' succeeded."
        }
        failure {
            mail to: 'your-email@example.com',
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed."
        }
    }
}

