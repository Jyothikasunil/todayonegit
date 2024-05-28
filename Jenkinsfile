pipeline {
    agent any

    environment {
        RECIPIENTS = 'jyothikasunil006@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the code...'
                    // Example using Maven
                    sh 'mvn clean package'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    // Example using Maven and JUnit
                    sh 'mvn test'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    // Example using SonarQube
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Example using OWASP Dependency-Check
                    sh 'dependency-check.sh --project MyProject --scan ./'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging...'
                    // Example using AWS CLI
                    sh 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name StagingGroup --s3-location bucket=mybucket,bundleType=zip,key=myapp.zip'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Example of running custom integration test script
                    sh './run-staging-tests.sh'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production...'
                    // Example using AWS CLI
                    sh 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name ProductionGroup --s3-location bucket=mybucket,bundleType=zip,key=myapp.zip'
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Pipeline finished. Sending notification emails...'
            }
        }
        success {
            emailext(
                to: env.RECIPIENTS,
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName} - SUCCESS",
                body: """<p>Build status: ${currentBuild.currentResult}</p>
                         <p>Check the console output at ${env.BUILD_URL} to view the results.</p>""",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: env.RECIPIENTS,
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName} - FAILURE",
                body: """<p>Build status: ${currentBuild.currentResult}</p>
                         <p>Check the console output at ${env.BUILD_URL} to view the results.</p>""",
                attachLog: true
            )
        }
    }
}
