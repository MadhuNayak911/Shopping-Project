pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK11'
    }

    environment {
        MVN_OPTS = '-B -V'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Cloning the repository...'
                git url: 'https://github.com/MadhuNayak911/Shopping-Project.git', branch: 'main'
            }
        }

        stage('Compile') {
            steps {
                echo '🛠 Compiling the code using Maven...'
                sh 'mvn $MVN_OPTS compile'
            }
        }

        stage('Unit Test') {
            steps {
                echo '🧪 Running unit tests...'
                sh 'mvn $MVN_OPTS test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Coverage') {
            steps {
                echo '📊 Generating code coverage report (JaCoCo)...'
                sh 'mvn $MVN_OPTS jacoco:report'
            }
            post {
                success {
                    publishHTML([reportDir: 'target/site/jacoco', reportFiles: 'index.html', reportName: 'JaCoCo Coverage'])
                }
            }
        }

        stage('Package') {
            steps {
                echo '📦 Packaging the application...'
                sh 'mvn $MVN_OPTS package'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, test, coverage and packaging succeeded!'
        }
        failure {
            echo '❌ Pipeline encountered an error.'
        }
    }
}
