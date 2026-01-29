pipeline {

    agent { label 'netligent' }
 
    stages {

        stage('Code Checkout') {

            steps {

                git branch: 'main', url: 'https://github.com/shikoh-zaidi/myfirstproject.git'

            }

        }
 
        stage('Build') {

            steps {

                sh 'docker build -t simpleapp:1 .'

            }

        }
 
        stage('Deploy') {

            steps {

                sh '''
                docker stop c1 || true
                docker rm c1 || true
                docker run -d -p 80:80 --name c1 simpleapp:1 --restart always
                '''

            }

        }

    }

}

 
