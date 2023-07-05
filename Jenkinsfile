pipeline{
    agent any
    

    stages{


         stage("Git Delivery"){
            steps{
                git branch: "master" , url: 'https://github.com/leoimewore/simple-java-project'
            }
            
        }


        stage("SonarQube Analysis"){
            steps{

                withSonarQubeEnv('sonar') {
                 sh 'mvn clean package sonar:sonar'
                }
            }   
            
        }

        stage("Quality Gate"){

            steps{
                waitForQualityGate abortPipeline: true, credentialsId: 'sonar'

                
            }
        }

         stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage("Tomcat Deploy") {

            steps{

                deploy adapters: [tomcat9(credentialsId: 'tomcat', 
                path: '', 
                url: 'http://3.129.217.110')], 
                contextPath: "reg", 
                war: "**/*.war"
                
            }
        }
    



    }


        


 


}
   