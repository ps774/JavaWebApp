pipeline {
    agent any
    tools{
        maven 'Maven3.8.3'
    }
    stages {
        stage("checkout") {
            steps {
               git credentialsId: 'd60190d7-b182-4f05-9f7c-f1defae5b9fc', url: 'https://github.com/ps774/JavaWebApp.git'
            }
        }
        stage("build") {
            steps {
                sh 'mvn clean install -f pom.xml'
            }
        }
        stage("CodeQuality") {
            steps {
                withSonarQubeEnv('SonarQube') {
                sh 'mvn sonar:sonar -f pom.xml'
                }
            }
        }
        stage("dev deploy") {
            steps {
                deploy adapters: [tomcat9(credentialsId: '4bf0f698-3771-473a-bd61-ede8d1a83571', path: '', url: 'http://34.227.66.240:8080/')], contextPath: 'Myweb001', war: 'target/*.war'
            }
        }
		stage('Dev apprl for QA') {
            steps {
                echo "taking approval from Dev Manager"
                timeout(time: 7, unit: 'DAYS') {
                    input message: 'Do you want to proceed to QA?', submitter:'admin'
                }
            }
        }
        stage('QA Deploy'){
            steps{
                deploy adapters: [tomcat9(credentialsId: '4bf0f698-3771-473a-bd61-ede8d1a83571', path: '', url: 'http://34.227.66.240:8080/')], contextPath: 'Myweb001', war: 'target/*.war'
            }
        }
        
    }
}
