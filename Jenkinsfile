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
                timeout(time: 90, unit: 'SECONDS') {
                    bat "cd .\\sample-project && mvn site:run -Dport=8081"
                }
            }
        }
    }
}
