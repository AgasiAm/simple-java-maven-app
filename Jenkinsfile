pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
		post {
                        always {
                		logstashSend failBuild: true, maxLines: 1000
                		}
		     }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                  junit 'target/surefire-reports/*.xml'
  		  logstashSend failBuild: true, maxLines: 1000
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
		 post {
                        always {
                		logstashSend failBuild: true, maxLines: 1000
                	}
		      }
        }
    }
}
