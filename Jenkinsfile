pipeline{
    agent any
      tools{
          maven 'Maven3.8.3'
      }
      
      stages{
          stage("Checkout"){
              steps{
                  git 'https://github.com/ps774/JavaWebApp.git'
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
                        deploy adapters: [tomcat9(credentialsId: '0e434691-6494-4b3b-95f7-5af7d27eb8ce', path: '', url: 'http://54.89.24.67:8080')], contextPath: null, war: 'target/*.war'
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
               deploy adapters: [tomcat9(credentialsId: '0e434691-6494-4b3b-95f7-5af7d27eb8ce', path: '', url: 'http://54.89.24.67:8080')], contextPath: null, war: 'target/*.war'
            }
        }
      }
}
