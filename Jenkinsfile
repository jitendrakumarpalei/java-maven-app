pipeline {
    agent any 
    tools {
        maven 'maven'
    }

    stages {
        stage('increment version') {
            steps {
                script {
                    echo "Increment version.."
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'   
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER" 
                }
            }
        }
        stage("build") {
            steps {
                script {
                    echo "building the application.."
                    sh "mvn clean package"
                }
            }
        }
        stage('build and push image') {
            steps {
                script {
                    echo "building the docker image.."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t jitendrapalei/java-maven-app:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push jitendrapalei/java-maven-app:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "deploying the docker image to EC2.."
                }
            }
        }
    }
}