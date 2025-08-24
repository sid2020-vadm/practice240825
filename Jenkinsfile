pipeline{
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Build') {
            steps {
                bat 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                scrpit{
                    catchError(buildResult:'UNSTABLE',stageResult:'FAILURE'){
                        withCredentials([usernamePassword(credentialsId: 'REST_BOOKER_CREDS', usernameVariable: 'RESTBOOKER_USERNAME', passwordVariable: 'RESTBOOKER_PASSWORD')]){
                            bat 'mvn clean install -Pcrud-tests'
                        }
                    }
                }
            }
        }
        stage('Reports') {
            steps{
                scrpit{
                    allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
                }
            }
        }

    }

}