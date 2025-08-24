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
                script{
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
                script{
                    allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
                }
            }
        }

    }

}