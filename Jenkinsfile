pipeline {
    agent any
    tools {
        maven 'maven-3.9.5'
    }
    
    stages {
        stage('clone') {
            steps {
                git 'https://github.com/jitendrakumarpalei/java-maven-app.git'
            }
        }
        stage('build') {
            steps {
                sh "mvn clean install"
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'

                }
            }
        }
    }
}
