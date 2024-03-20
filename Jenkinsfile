pipeline {
    agent any
    
    environment {
        APP_PORT = '9090'
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Integration Test') {
            parallel {
                stage('Running Application') {
                    agent any
                    options {
                        timeout(time: 60, unit: 'SECONDS')
                    }
                    steps {
                        script {
                            try {
                                dir("target") {
                                    sh 'java -jar contact.war'
                                }
                            } catch (Exception e) {
                                echo 'success'
                            }
                        }
                    }
                }
                stage('Running Test') {
                    steps {
                        script {
                            sh 'sleep 30' 
                            sh 'mvn test -Dtest=RestIT'
                        }
                    }
                }
            }
        }
    }
}

