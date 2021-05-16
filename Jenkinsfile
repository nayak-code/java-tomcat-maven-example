pipeline {
    agent any
    environment {
        PATH = "/opt/maven/bin:$PATH"
    }
    stages
    {
        stage('SCM')
        {
            steps
            {
                git 'https://github.com/daticahealth/java-tomcat-maven-example'
            }
        }
        stage('Buid')
        {
            steps
            {
                sh 'mvn clean package'
            }
        }
        stage('Test')
        {
            steps
            
            {
                sh 'mvn test'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults : 'target/*.xml'
                }
            }
        }
        stage('Code Quality Test')
        {
            steps
            {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Deploy on Tomcat')
        {
            steps
            {
               sh 'cp /var/lib/jenkins/workspace/Project-1/target/java-tomcat-maven-example.war /opt/tomcat/webapps'
            }
        }
    }
}
