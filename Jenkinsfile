pipeline {
    agent any
    tools{
        maven 'Maven3.8.3'
    }
    stages {
        stage("checkout") {
            steps {
               git credentialsId: '1bec3b7c-9b47-4019-8f26-c8ab4d8d4d25', url: 'https://github.com/ps774/JavaWebApp.git'
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
                sh 'mvn clean sonar:sonar -f pom.xml'
                }
            }
        }
        stage("dev deploy") {
            steps {
                deploy adapters: [tomcat9(credentialsId: '8701b095-1eb1-40e1-aa59-47ac0286a12a', path: '', url: 'http://52.91.166.237:8080')], contextPath: 'app', war: 'target/*.war'
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
               deploy adapters: [tomcat9(credentialsId: '8701b095-1eb1-40e1-aa59-47ac0286a12a', path: '', url: 'http://52.91.166.237:8080')], contextPath: 'app', war: 'target/*.war'
            }
        }
        
    }
}
