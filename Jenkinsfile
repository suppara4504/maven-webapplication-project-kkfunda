/*
node {

    // START NOTIFICATION
    slackSend channel: '#shivakuu',
              message: """🚀 *Build STARTED*
*Job:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*Job URL:* ${env.JOB_URL}
*Build URL:* ${env.BUILD_URL}
""",
              color: '#439FE0'

    try {

        def mavenHome = tool name: "maven"

        stage('Git Checkout') {
            git branch: 'Dev',
                url: 'https://github.com/suppara4504/maven-webapplication-project-kkfunda.git'
        }

        stage('Maven Compile') {
            sh "${mavenHome}/bin/mvn compile"
        }

        stage('Build') {
            sh "${mavenHome}/bin/mvn clean package"
        }

        stage('SQ Report') {
            sh "${mavenHome}/bin/mvn sonar:sonar"
        }

        stage('Nexus Deploy') {
            sh "${mavenHome}/bin/mvn clean deploy"
        }

        stage('Deploy to Tomcat') {
            sh """
                curl -u admin:shivakuu \
                --upload-file /var/lib/jenkins/workspace/Scripted-way-pipeline/target/maven-web-application.war \
                "http://65.0.61.147:8080/manager/text/deploy?path=/maven-web-application&update=true"
            """
        }

        // SUCCESS NOTIFICATION
        slackSend channel: '#shivakuu',
                  message: """🟢 *Build SUCCESS*
*Job:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*Job URL:* ${env.JOB_URL}
*Build URL:* ${env.BUILD_URL}
""",
                  color: 'good'

    } catch (err) {

        // FAILURE NOTIFICATION
        slackSend channel: '#shivakuu',
                  message: """🔴 *Build FAILED*
*Job:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*Error:* ${err}
*Job URL:* ${env.JOB_URL}
*Build URL:* ${env.BUILD_URL}
""",
                  color: 'danger'

        throw err

    } finally {

        // ALWAYS END
        slackSend channel: '#shivakuu',
                  message: """📦 *Build FINISHED* (Success or Failure)
*Job:* ${env.JOB_NAME}
*Build:* #${env.BUILD_NUMBER}
*Job URL:* ${env.JOB_URL}
*Build URL:* ${env.BUILD_URL}
"""
    }

}  // END NODE
*/
pipeline {

    agent any

    tools {
        maven "maven"
    }

    options {
        timestamps()   // Optional but helpful
    }

    // 🔔 Send START Notification

    stages {

        stage('Start Notification') {
            steps {
                slackSend channel: '#shivakuu',
                          message: "🚀 *Build STARTED*\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}",
                          color: '#439FE0'
            }
        }

        stage('Git Checkout') {
            steps {
                git branch: 'Dev',
                    url: 'https://github.com/suppara4504/maven-webapplication-project-kkfunda.git'
            }
        }

        stage('Maven Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

         stage('SQ Report') {
            steps {
                sh 'mvn sonar:sonar'
            }
        } 

        stage('Nexus Integration') {
            steps {
                sh 'mvn clean deploy'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    curl -u admin:shivakuu \
                    --upload-file /var/lib/jenkins/workspace/Declarative-way/target/maven-web-application.war \
                    "http://3.110.207.121:8080/manager/text/deploy?path=/maven-web-application&update=true"
                '''
            }
        }
        stage('bsnl-qa') {
          steps{
              build job: 'bsnl-qa'
          }
        }
    }

    // 🔥 Post section handles SUCCESS, FAILURE, ALWAYS
    post {

        success {
            slackSend channel: '#shivakuu',
                      message: "🟢 *Build SUCCESS*\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}",
                      color: 'good'
        }

        failure {
            slackSend channel: '#shivakuu',
                      message: "🔴 *Build FAILED*\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}",
                      color: 'danger'
        }

        always {
            slackSend channel: '#shivakuu',
                      message: "📦 *Build FINISHED* (Success or Failure)\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nURL: ${env.BUILD_URL}"
        }
    }
}

