pipeline {
    agent any
    tools {
        maven "MAVEN"
        jdk "OpenJDK17"
    }

    environment {
        SNAP-REPO= 'vprofile-snapshot'
        NEXUS-USER= 'admin'
        NEXUS-PASS= 'admin'
        RELEASE-REPO= 'vprofile-release'
        CENTRAL-REPO= 'vpro-maven-central'
        NEXUSIP= '172.31.41.93'
        NEXUSPORT= '8081'
        NEXUS-GRP-REPO= 'vpro-maven-group'
        NEXUS-LOGIN= 'nexuslogin'
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
}