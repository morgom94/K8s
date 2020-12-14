pipeline {
    agent any
    tools {
        maven "mavenTool"
    }
    parameters {
        choice(choices: ['TestMagicBuilder', 'TestMessageBuilder'], description: 'Choose a test to buildtr', name: 'TESTS')
    }
    stages {
        stage('Build') {
            steps {
                git 'https://github.com/morgom94/Maven.git'
                bat "mvn clean package -DskipTests"
            }
        }
        stage('Tests') {
            steps {
                bat "mvn test"
            }
            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('One Test') {
            steps {
                bat "mvn -Dtest=${params.TESTS} test"
            }
        }
    }
}
