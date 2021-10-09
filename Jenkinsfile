pipeline{
    agent any
      tools{
          maven 'Maven3.8.3'
      }
      
      stages{
          stage("Checkout"){
              steps{
                  git credentialsId: '1bec3b7c-9b47-4019-8f26-c8ab4d8d4d25', url: 'https://github.com/ps774/JavaWebApp.git'
              }
            }  
              stage("build"){
                  steps{
                      sh 'mvn clean install -f pom.xml'
                  }
              }
                    
                stage("CodeQuality"){
                    steps{
                        withSonarQubeEnv('SonarQube'){
                        sh 'mvn sonar:sonar -f pom.xml'
                        }
                    }
                }          
                stage("Deploy"){
                    steps{
                        deploy adapters: [tomcat9(credentialsId: '4df4a4f8-990f-4d5e-8f6b-b74a66649824', path: '', url: 'http://54.174.2.36:8080/manager/html')], contextPath: null, war: '**/*.war'
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
               deploy adapters: [tomcat9(credentialsId: '4df4a4f8-990f-4d5e-8f6b-b74a66649824', path: '', url: 'http://54.174.2.36:8080/manager/html')], contextPath: null, war: '**/*.war'
            }
        }
      }
}
