pipeline {
    agent any
    tools {
        maven 'Maven 3.6.0'
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
                    sh 'mvn -Dmaven.test.failure.ignore=true install'
                }
            }
            post {
                success {
                    dir('complete') {
                        junit 'target/surefire-reports/**/*.xml'
                        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true, allowEmptyArchive: true
                        archiveArtifacts artifacts: 'target/*.war', fingerprint: true, allowEmptyArchive: true
                    }
                }
            }
        }
    }
}