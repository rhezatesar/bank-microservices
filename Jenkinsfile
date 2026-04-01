pipeline {
    agent any

    stages {

        stage('Account Service Scan') {
            steps {
                dir('account-service') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Transaction Service Scan') {
            steps {
                dir('transaction-service') {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('DB Migration Account') {
            steps {
                sh '''
                docker run --rm                 --network jenkins_network                 -v $PWD/account-service/db/migration:/flyway/sql                 flyway/flyway                 -url=jdbc:postgresql://postgres:5432/sonarqube                 -user=sonar                 -password=sonar123                 migrate
                '''
            }
        }

        stage('DB Migration Transaction') {
            steps {
                sh '''
                docker run --rm                 --network jenkins_network                 -v $PWD/transaction-service/db/migration:/flyway/sql                 flyway/flyway                 -url=jdbc:postgresql://postgres:5432/sonarqube                 -user=sonar                 -password=sonar123                 migrate
                '''
            }
        }
    }
}
