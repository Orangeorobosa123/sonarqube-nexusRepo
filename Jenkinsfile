pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar_server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
             nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'WAR']], credentialsId: 'nexus', groupId: 'SampleWebApp', nexusUrl: 'ec2-44-201-86-30.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOTS'
            
        }
        
        stage('deploy to tomcat') {
          steps {
             deploy adapters: [tomcat9(credentialsId: 'tomcat_id', path: '', url: 'http://54.210.117.188:8080/')], contextPath: 'webapp', war: '**/*.war'
              
              
              
          }
            
        }
            
        }
} 
