pipeline {
    agent any
    tool name: 'mymaven', type: 'maven'
    stages
    {
        stage('SCM')
        {
            steps
            {
                git 'https://github.com/nayak-code/java-tomcat-maven-example.git'
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
    }
}
