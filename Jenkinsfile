pipeline {
    agent any
    tools { maven 'mymaven' }	
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
		echo "The build number is ${env.BUILD_NUMBER}"
		echo "You can also use \${BUILD_NUMBER} -> ${BUILD_NUMBER}"
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
