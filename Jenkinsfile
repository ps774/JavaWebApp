
pipeline{
    agent any
      tools{
          maven 'Maven3.8.3'
      }
      
      stages{
          stage("Checkout"){
              steps{
                  git credentialsId: '25cc084e-fde3-4122-9437-d21d336816ad', url: 'https://github.com/ps774/JavaWebApp.git'
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
                        deploy adapters: [tomcat9(credentialsId: 'fe62a633-96e3-493e-bb0c-3a24dbc81e7e', path: '', url: 'http://18.206.147.101:8080')], contextPath: null, war: 'target/*.war'
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
               deploy adapters: [tomcat9(credentialsId: 'fe62a633-96e3-493e-bb0c-3a24dbc81e7e', path: '', url: 'http://18.206.147.101:8080')], contextPath: null, war: 'target/*.war'
            }
        }
      }
}
