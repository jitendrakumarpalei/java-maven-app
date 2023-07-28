#!/usr/bin/env groovy

@Library('jenkins-shared-library')

def gv


pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('init') {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage('build jar') {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage('build push image') {
            steps {
                script {
                    buildImage('jitendrapalei/java-maven-app:3.0')
                    dockerLogin()
                    dockerPush('jitendrapalei/java-maven-app:3.0')
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}