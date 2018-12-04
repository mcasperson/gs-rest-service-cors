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

        stage('Deploy - Staging') {
                    steps {
                        withCredentials([string(credentialsId: 'TomcatPassword', variable: 'TomcatPassword'),
                                string(credentialsId: 'TomcatUsername', variable: 'TomcatUsername'),
                                string(credentialsId: 'TomcatUrlStaging', variable: 'TomcatUrl')]) {
                            TOMCAT_USERNAME=${TomcatUsername} TOMCAT_PASSWORD=${TomcatPassword} TOMCAT_URL=${TomcatUrlStaging} mvn tomcat7:deploy
                        }
                    }
                }

        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }

        stage('Deploy - Production') {
            steps {
                withCredentials([string(credentialsId: 'TomcatPassword', variable: 'TomcatPassword'),
                        string(credentialsId: 'TomcatUsername', variable: 'TomcatUsername'),
                        string(credentialsId: 'TomcatUrlProduction', variable: 'TomcatUrl')]) {
                    TOMCAT_USERNAME=${TomcatUsername} TOMCAT_PASSWORD=${TomcatPassword} TOMCAT_URL=${TomcatUrlProduction} mvn tomcat7:deploy
                }
            }
        }
    }
}