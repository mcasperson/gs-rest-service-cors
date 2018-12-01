pipeline {
    agent any
    stages {
        stage ('Build') {
            steps {
                dir('complete') {
                    sh './mvnw -Dmaven.test.failure.ignore=true install'
                }
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
    }
}