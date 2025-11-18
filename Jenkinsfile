pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    stages {


        stage('Build Backend') {
            steps {
                dir('demo-app') {
                    sh "mvn clean install -DskipTests"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    dir('demo-app') {
                        sh '''
                            mvn clean verify sonar:sonar                               -Dsonar.projectKey=demo-app                              -Dsonar.host.url=$SONAR_HOST_URL                               -DskipTests
                        '''
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                script {
                    timeout(time: 3, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}