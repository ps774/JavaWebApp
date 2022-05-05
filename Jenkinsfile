
pipeline{
    agent any
      tools{
          maven 'Maven3.8.5'
      }
      
      stages{
          stage("Checkout"){
              steps{
                  git credentialsId: '25cc084e-fde3-4122-9437-d21d336816ad', url: 'https://github.com/ps774/JavaWebApp.git'
              }
            }  
              stage("build"){
                  steps{
                      sh 'mvn clean install'
                  }
              }
                    
                stage("CodeQuality"){
                    steps{
                        withSonarQubeEnv('SonarJenkins'){
                        sh 'mvn sonar:sonar'
                        }
                    }
                }          
                
      }
}
