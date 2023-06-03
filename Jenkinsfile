pipeline {
    agent any
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "localhost:8081"
        NEXUS_REPOSITORY = "Repositorio-Pipeline"
        NEXUS_CREDENTIAL_ID = "Admin-Nexus"
        
    }

    stages {
        stage('Initialize'){
            steps{
                echo "Acá comienza"
            }
        }
    
      stage('Build'){
            steps {
                //sh 'mvn -B package'
                sh 'mvn -B -DskipTests clean package'
            }
        }

       /*   stage('Test') { 
            steps {
                sh 'mvn clean verify' 
            }
        }*/

        stage('SonarQube analysis') {
            environment {
                SCANNER_HOME = tool 'SonarQube Scanner installer'
            }
            steps {
                echo "Acá comienza Sonnar"
                withSonarQubeEnv(credentialsId: 'Token-Sonar-2', installationName: 'SonarQube') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=Maven-Pipeline \
                    -Dsonar.projectName=Maven-Pipeline \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/classes/ \
                    -Dsonar.exclusions=src/test/java/****/*.java \
                    -Dsonar.projectVersion=${BUILD_NUMBER}-${GIT_COMMIT_SHORT}'''
                }
            }
        }

        stage("Publish to Local Nexus Repository Manager") {
            steps {
                echo "Acá comienza Nexus"
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }    
    } 
     post {
        always{
            slackSend( channel: "#grupo4", token: "zrR2KrxeHjCxLqKIxXg2QfDf", color: "good", message: "Test-Pipeline")
                           
             }
             }         
   
}
//FIN
//Otro 
