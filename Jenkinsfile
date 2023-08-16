pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage("Test") {
            steps {
                script {
                    try {
                        sh "mvn --version"
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error "Test stage failed: ${e.getMessage()}"
                    }
                }
            }
        }
        stage("Build") {
            steps {
                script {
                    try {
                        sh "mvn package"
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error "Build stage failed: ${e.getMessage()}"
                    }
                }
            }
        }
        stage("Deploy-Test") {
            steps {
                script {
                    try {
                        deploy adapters: [tomcat9(credentialsId: 'tomcatdetail', path: '', url: 'http://54.80.106.102:8080/')], contextPath: '/app', war: '**/*.war'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error "Deploy-Test stage failed: ${e.getMessage()}"
                    }
                }
            }
        }
        stage("Deploy-Prod") {
            steps {
                script {
                    try {
                        deploy adapters: [tomcat9(credentialsId: 'tomcatdetail', path: '', url: 'http://34.228.230.211:8080/')], contextPath: '/app', war: '**/*.war'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error "Deploy-Prod stage failed: ${e.getMessage()}"
                    }
                }
            }
        }
    }
    post {
        always {
            echo "========always========"
        }
        success {
            echo "========pipeline executed successfully ========"
            emailext body: "Pipeline executed successfully.",
                recipientProviders: [[$class: 'CulpritsRecipientProvider']],
                subject: "Pipeline Success",
                to: "your-email@example.com"
        }
        failure {
            echo "========pipeline execution failed========"
            emailext body: "Pipeline execution failed. Please check the logs for more details.",
                recipientProviders: [[$class: 'CulpritsRecipientProvider']],
                subject: "Pipeline Failure",
                to: "your-email@example.com"
        }
    }
}
