pipeline {
    agent any
    tools {
        maven "MAVEN"
        jdk "OpenJDK17"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '172.31.41.93'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
    }

    stages{
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }
        }
        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Sonar Analysis'){
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps{
                withSonarQubeEnv("${SONARSERVER}") {
                    sh '''${scannerHome}/bin/sonar-scanner -X -Dsonar.projectKey=vprofile -Dsonar.login=b18ea1d605f11f662f8525f98ba41b2627432895 -Dsonar.projectName=vprofile-repo -Dsonar.projectVersion=1.0 -Dsonar.sources=/var/lib/jenkins/workspace/vprofile-ci-pipeline/src/ -Dsonar.java.binaries=/var/lib/jenkins/workspace/vprofile-ci-pipeline/target/test-classes/com/visualpathit/account/controllerTest/ -Dsonar.junit.reportsPath=/var/lib/jenkins/workspace/vprofile-ci-pipeline/target/surefire-reports/ -Dsonar.jacoco.reportsPath=/var/lib/jenkins/workspace/vprofile-ci-pipeline/target/jacoco.exec -Dsonar.java.checkstyle.reportPaths=/var/lib/jenkins/workspace/vprofile-ci-pipeline/target/checkstyle-result.xml'''
                }
            }
        }
    }
}