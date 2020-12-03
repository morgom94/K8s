pipeline {
    agent any
    tools {
        maven "mavenTool"
    }
    environment{
        MY_FILE = fileExists '.\\sample-project'
        GIT_PASSWORD = 'Mogea.110994'
        GIT_USERNAME = 'morgom94'
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
                catchError(buildResult: 'ABORTED', message: 'stage completed', stageResult: 'SUCCESS'){
                    timeout(time: 10, unit: 'SECONDS') {
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
    }
}
