pipeline {
    agent any
    
    tools {
        maven 'Maven_Home'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "mvn clean install -f MyAwesomeApp/pom.xml"
                }
            }
        }

        stage('Code Quality Scan') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        sh "mvn sonar:sonar -f MyAwesomeApp/pom.xml"
                    }
                }
            }
        }

        stage('DEV Deploy') {
            steps {
                script {
                    echo "deploying to DEV Env"
                    deploy adapters: [tomcat8(credentialsId: 'da622b00-f7f3-4a3a-b4fc-985bdfe6000b', path: '', url: 'http://34.205.24.116:8080')], contextPath: null, war: '**/*.war'
                }
            }
        }

        stage('DEV Approve') {
            steps {
                input message: 'Do you want to deploy?', submitter: 'admin'
            }
        }

        stage('Slack Notification') {
            steps {
                slackSend(channel: 'myawesomeapp', message: "Job is successful, here is the info - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            }
        }
    }
}

