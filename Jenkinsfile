pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven-3"
        jdk "jdk-17"
    }

    stages {
        stage('Build') {
            when { tag "BUILD_TAG*" }
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/remaj94/szkolenie-ci-jenkins-example.git'
            }
        }
        stage('Clean') {
                steps {
                     // Run Maven on a Unix agent.
                    sh 'mvn clean verify'
                }
            }
        }
  post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                     slackSend(channel: "jenkins-mikolaj-skapski", message: "Build Success - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)")
                }
                failure {
                     slackSend(channel: "jenkins-mikolaj-skapski", message: "Build Failure - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)")
                }
  }
}
