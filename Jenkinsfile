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
              nexusArtifactUploader artifacts: [[artifactId: 'maven-snapshots', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexus', groupId: 'SampleWebApp', nexusUrl: 'ec2-44-201-86-30.compute-1.amazonaws.com:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: '1.0-SNAPSHOT', version: ''
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
             deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://34.228.168.182:8080')], contextPath: 'myapp', war: '**/*.war'
              
              
              
          }
            
        }
            
        }
} 
