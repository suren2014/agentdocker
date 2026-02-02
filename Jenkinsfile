pipeline {
    agent { label "${LABEL_NAME}" }

    environment {
        IMAGE_NAME = "netli"
        IMAGE_TAG  = "${BUILD_NUMBER}"
        DOCKER_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/suren2014/agentdocker.git'
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop c1 || true
                    docker rm c1 || true
                    docker run -d \
                      --name c1 \
                      -p 80:80 \
                      --restart always \
                      ${DOCKER_IMAGE}
                '''
            }
        }
    }

    post {
        failure {
            emailext(
                subject: "Build FAILED ${BUILD_NUMBER}",
                body: """This email is regarding the failed build.
Check console output of ${BUILD_NAME}""",
                to: "mandawala.net@gmail.com"
            )
        }
    }
}
