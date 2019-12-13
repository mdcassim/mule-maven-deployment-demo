pipeline {
    agent {
     node { 
        label 'node1'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
		stage('SonarQube analysis') {
			steps {
				echo "SonarQube Stage"
			sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar'
			}
		}
		stage('Test') {
            steps {
                echo "Middle Stage"
            }
           // post {
             //   always {
                   // junit 'target/surefire-reports/*.xml'
              //  }
         //   }
        }
		stage('Deliver') {
            steps {
				echo "Testing phase"
                 sh 'curl -uadmin:AP5r3rBQ9jvLqtQkCihjHKafUhq -T /home/jenkins/node/workspace/Mule_CICD_Abhishek/target/test1-1.0.0-SNAPSHOT-mule-application.jar "http://mdcassimsait.southindia.cloudapp.azure.com:8081/artifactory/example-repo-local/$BUILD_NUMBER/test1-1.0.0-SNAPSHOT-mule-application.zip"'
			}
        }
        stage('Deploy Test') {
            steps { 
                sh 'mvn mule:deploy -P cloudhub -Dmule.version=3.9.0 -Dmule.artifact="http://mdcassimsait.southindia.cloudapp.azure.com:8081/artifactory/example-repo-local/$BUILD_NUMBER/test1-1.0.0-SNAPSHOT-mule-application.jar" -Danypoint.username=ankitshastri05 -Danypoint.password=Ashu52824@'
                      }
                    }
                }
            }
        }
        }
}
