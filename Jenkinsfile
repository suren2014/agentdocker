pipeline {
    agent { label "${LABEL_NAME}" }
    environment {
        IMAGE_NAME = "netli"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/suren2014/agentdocker.git', branch:"main"
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}."
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop c1 || true
                docker rm c1 || true
                docker run -d -p 80:80 --name c1 -P 80:80 --restart always ${DOCKER_IMAGE}
                '''
            }
        }
    }
    post {
        emailext {
        body: '''This email is regarding the failed build.
Check console output of ${BUILD_NAME}''', subject: 'Build FAILED ${BUILD_NUMBER}', to: 'mandawala.net@gmail.com'
        }
    }
}
