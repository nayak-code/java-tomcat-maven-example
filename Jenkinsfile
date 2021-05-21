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
	stage('SQ_QualityGateCheck')
        {
	    steps
	    {
	       script 
	       {
	        timeout(time: 1, unit: 'MINUTES') {
			def qg = waitForQualityGate()
		        if (qg.status != 'OK') {
			    error "Pipeline aborted due to quality gate failure: ${qg.status}"
			}
		}
	       }
	    }
	}
        stage('Deploy on Tomcat')
        {
            steps
            {
               sh 'cp /root/.jenkins/workspace/Project-1/target/java-tomcat-maven-example.war /opt/tomcat/webapps'
            }
        }
    }
}
