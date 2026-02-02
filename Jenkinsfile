pipeline {

    agent { label "${LABEL_NAME}" }

    environment {
        IMAGE_NAME   = "netli"
        IMAGE_TAG    = "${BUILD_NUMBER}"
        DOCKER_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {

        stage('Code Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shikoh-zaidi/myfirstproject.git'
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
                      -p 80:80 \
                      --name c1 \
                      --restart always \
                      ${DOCKER_IMAGE}
                '''
            }
        }
    }
    post {
        emailext ( body: '''This mail is regarding failed build ${BUILD_NUMBER}', 
            subject: 'Buils Failed ${BUILD_NUMBER}''', 
            to: 'shikoh.zaidi@live.com'
                 )
    }
}
