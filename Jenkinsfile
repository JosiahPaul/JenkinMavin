pipeline {
    agent any
    
    stages {
        stage('Git Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], 
                extensions: [], userRemoteConfigs: [[url: 'https://github.com/JosiahPaul/JenkinMavin.git']]])
            }
        }
            
        stage('Scan Code with Mysonar') {
            steps {
                withSonarQubeEnv('mysonar') {
                 sh "mvn -f SampleWebApp/pom.xml sonar:sonar"   
                }
            }
        }
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
        stage('Test with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        }
    
        stage('deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: '75286872-a9cf-4650-8fed-17adedf9efe0', 
                path: '', url: 'http://52.23.247.195:8080/')], contextPath: 'josh', war: '**/*.war'
            }
        }
        
    }
}
