pipeline {
    agent any
    tools {
        maven 'maven' // Use lowercase 'maven' instead of 'Maven'
    }
    stages {
        stage("Test") {
            steps {
                sh "mvn --version"
            }
        }
        stage("Build") {
            steps {
                sh "mvn package"
            }
        }
        stage("Deploy-Test") {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcatdetail', path: '', url: 'http://54.80.106.102:8080/')], contextPath: '/app', war: '**/*.war'
            }
        }
        stage("Deploy-Prod") {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcatdetail', path: '', url: 'http://34.228.230.211:8080/')], contextPath: '/app', war: '**/*.war'
            }
        }
    }
    post {
        always {
            echo "========always========"
        }
        success {
            echo "========pipeline executed successfully ========"
        }
        failure {
            echo "========pipeline execution failed========"
        }
    }
}
