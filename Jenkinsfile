pipeline{
    
    agent any 
    
  
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/ramanasamineedi/demo-counter-app.git'
                }
            }  
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
         stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                      withSonarQubeEnv(credentialsId: 'sonar-key') {     
                        sh 'mvn clean package sonar:sonar'
                        }
                   }
                    
                }
            }
        stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-key'
                    }
                }
            }
        stage('nexus arifact'){
            
            steps{
                
                script{
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                        ]
                    ], 
                        credentialsId: '4eb3b103-c99a-4859-815e-bea48e4230e2', 
                        groupId: 'com.example', 
                        nexusUrl: '54.214.215.135:8081/', 
                        nexusVersion: 'nexus2', 
                        protocol: 'http', 
                        repository: 'http://54.214.215.135:8081/repository/demo-snapshot/', 
                        version: '1.0.0'
                }
            }
        }  
     }
}
