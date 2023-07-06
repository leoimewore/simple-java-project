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


        stage("Nexus Artifactory") {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'works-with-heroku',
                classifier: '', file: 'target/works-with-heroku-1.0.war',
                type: "war"]],
                credentialsId: 'nexus',
                groupId: 'works.buddy.samples', 
                nexusUrl: '18.191.164.194:8081', 
                nexusVersion: 'nexus2', 
                protocol: 'http', 
                repository: 'HerokuProject', 
                version: '1.0'
            }
        }

        stage("Tomcat Deploy") {

            steps{

                deploy adapters: [tomcat9(credentialsId: 'tomcat', 
                path: '', 
                url: 'http://3.129.217.110:8080')], 
                contextPath: "mypath", 
                war: "**/*.war"
                
            }
        }
    



    }


        


 


}
   