pipeline {
    agent any
    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '2', daysToKeepStr: '2'))
        disableConcurrentBuilds()
    }

    tools {
        jdk 'JAVA'
        maven 'maven'
    }

    stages {
        stage ('maven build') {
            steps {
                bat 'mvn clean package -f ./app/pom.xml'
            }
        }

        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    // Optionally use a Maven environment you've configured already
                    withMaven(maven:'maven') {
                        bat 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
    }
}