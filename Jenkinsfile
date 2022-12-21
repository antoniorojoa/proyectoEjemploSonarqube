pipeline {
    agent any
    tools {
      maven 'miMaven'
    }

    stages {
        stage('Compilacion') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Compilacion de pruebas unitarias') {
            steps {
                sh 'mvn test-compile'
            }
        }
        stage('Pruebas') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'mvn test'
                }
            }
            post{
                always {
                    sh 'mvn jacoco:report'
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn sonar:sonar'
                    }
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
                failure {
                    error 'Fallo en los test unitarios'
                }
            }
        }
    }
}