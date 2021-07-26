pipeline {
    agent any
    tools {
        nodejs 'Node 8.0.0'
    }
    stages {
        stage('build') {
            steps {
                withNPM(npmrcConfig:'npmrc-github') {
					sh 'npm install'
				}
            }
        }
        stage('test') {
            steps {
                sh 'npm test'
            }
        }
        stage('publish [global npm]') {
            when {
                branch "master"
            }
            steps {
                withNPM(npmrcConfig:'npmrc-github') {
					sh 'npm publish'
				}
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        failure {
            slackSend color: 'danger', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        }
        success {
            slackSend color: 'good', message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        }
        unstable {
            slackSend color: 'warning', message: "UNSTABLE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        }
    }
}