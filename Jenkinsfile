pipeline {
    agent any
    tools {
        maven 'MAVEN'
    }

    stages {

        stage('Build Maven') {
            steps{
                 git branch: 'main', credentialsId: 'devopshint', url: 'https://github.com/devopshint/jenkins-nexus'
                 sh "mvn -Dmaven.test.failure.ignore=true clean package"
                
            }
        }   
   stage("Publish to Nexus Repository Manager") {

            steps {

                script {

                    pom = readMavenPom file: "pom.xml";

                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");

                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"

                    artifactPath = filesByGlob[0].path;

                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {

                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                        nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            
                            protocol: 'http',

                            nexusUrl: '52.66.46.76:8081/',

                            groupId: 'pom.com.mycompany.app',

                            version: 'pom.1.0-SNAPSHOT',

                            repository: 'repository/maven-central-repository',

                            credentialsId: 'NEXUS_CRED',

                            artifacts: [

                                [artifactId: 'pom.my-app',

                                classifier: '',

                                file: artifactPath,

                                type: pom.packaging],

                                [artifactId: 'pom.my-app',

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
}
