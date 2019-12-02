pipeline {
    agent {
     node { 
        label 'node1'
        }
    }
    stages {
        stage('Build') {
            steps { 
               
                    
                    sh 'mvn -s $MAVEN_SETTINGS clean package -DskipTests=true'
            }
        }
        stage('Deploy Test') {
            steps { 
                configFileProvider(
                    [configFile(fileId: 'mulesoft-maven-settings', variable: 'MAVEN_SETTINGS')]) {
                    withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                               withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'mule-builder', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                      sh 'mvn -s $MAVEN_SETTINGS -Ddeploy.username=$USERNAME -Ddeploy.password=$PASSWORD deploy -DskipTests=true'
                      }
                    }
                }
            }
        }
        stage('Upload Artifact') {
            steps {
                withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                                 script {
                                        def server = Artifactory.newServer url: 'http://<artifactory url>:8081/artifactory', credentialsId: 'mulesoft-artifactory'
                                        server.bypassProxy = true
                                        def buildInfo = server.upload spec: uploadSpec
                                        }
                    }
                }
            }
        }
}
