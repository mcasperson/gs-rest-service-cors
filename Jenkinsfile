pipeline {
    agent any
    tools {
        jdk 'jdk8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                dir('complete') {
                    sh './mvnw -Dmaven.test.failure.ignore=true install'
                }
            }
            post {
                success {
                    dir('complete') {
                        junit 'target/surefire-reports/**/*.xml'
                    }
                }
            }
        }
    }
}