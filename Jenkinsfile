pipeline {
    options {
        
        timeout(time: 60, unit: 'MINUTES')
    }    
    agent {node {label 'Linux'}}
    environment {
        //TODO: Customize these variables for your environment
        MAVEN_PATH = "/myvagrant/apache-maven-3.8.4/bin"
        STAGE_PROJECT = "cars-stage"
        APP_GIT_URL = "https://github.com/VictorinCondei/JavaExamples.git"
        NEXUS_SERVER = "http://nexus-common.apps.cluster.domain.example.com/repository/java"

        // DO NOT CHANGE THE GLOBAL VARS BELOW THIS LINE
        APP_NAME = "cars"
    }    
    stages {
        stage('Static Code Analysis') {
            steps {
                echo '### Checking Static Code Style  ###'
                sh '''
                        export PATH=${MAVEN_PATH}:$PATH
                        pwd
                        mvn clean checkstyle:check
                   '''
            }
        }
        stage('Run Unit Tests') {
            steps {
                echo '### Running unit tests ###'
                sh '''
                        export PATH=${MAVEN_PATH}:$PATH
                        mvn clean test
                   '''
            }
        }

        stage('Create WAR file') {
            steps {
                echo '### Creating fat JAR ###'
                sh '''
                        export PATH=${MAVEN_PATH}:$PATH
                        mvn clean package -DskipTests=true
                   '''
            }
        }
        stage('Promote to Staging Env') {
            steps {
                timeout(time: 60, unit: 'MINUTES') {
                    input message: "Promote to Staging?"
                }

            }
        }

    }
}