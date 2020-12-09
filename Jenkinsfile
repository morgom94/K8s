pipeline {
    agent any
    tools {
        maven "mavenTool"
    }
    environment{
        MY_FILE = fileExists '.\\sample-project'
    }
    
    stages {
        stage('Project created') {
            when { expression { MY_FILE == 'true' } }
            steps {
                echo "Project already created !!! "
            }
        }
        stage('Generating project'){
            when { expression { MY_FILE == 'false' } }
            steps {
                echo "Creating new project !!!"
                bat "mvn archetype:generate -DgroupId=org.sonatype.mavenbook -DartifactId=sample-project"
            }
        }
        stage("Site") {
            steps {
                catchError(buildResult: 'ABORTED', message: 'STAGE COMPLETED', stageResult: 'SUCCESS'){
                    timeout(time: 100, unit: 'SECONDS') {
                        bat "cd .\\sample-project && mvn site:run -Dport=8081"
                    }
                }
            }
        }
        stage("Jar") {
            steps {
                bat "cd .\\sample-project && mvn site:jar"
                echo "JAR CONTENT"
                bat "jar -tf .\\sample-project\\target\\sample-project-1.0-SNAPSHOT-site.jar"
            }
        }
        stage("Deploy") {
            steps {
                bat "cd .\\sample-project && mvn site:deploy"
            }
        }
        stage("java cp"){
            steps{
                bat "java -cp .\\sample-project\\target\\sample-project-1.0-SNAPSHOT.jar org.sonatype.mavenbook.App"
            }
        }
    }
}
