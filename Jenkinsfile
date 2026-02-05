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
        success {
            archiveArtifacts: '*.tar'
            emailext(
                subject: "Build Success: ${JOB_NAME} #${BUILD_NUMBER}",
                body: """
Hello Team,

The Jenkins build completed successfully.

Job Name : ${JOB_NAME}
Build No : ${BUILD_NUMBER}
Docker Image : ${DOCKER_IMAGE}
Build URL: ${BUILD_URL}

Regards,
Jenkins
""",
                to: 'shikoh.zaidi@live.com'
            )
        }

        failure {
            emailext(
                subject: "Build Failed: ${JOB_NAME} #${BUILD_NUMBER}",
                body: """
Hello Team,

The Jenkins build has failed.

Job Name : ${JOB_NAME}
Build No : ${BUILD_NUMBER}
Build URL: ${BUILD_URL}

Please check the logs for details.

Regards,
Jenkins
""",
                to: 'shikoh.zaidi@live.com'
            )
        }
    }
}
