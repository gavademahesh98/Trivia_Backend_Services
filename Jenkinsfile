pipeline {

    agent any

    environment {
        DOCKERIMG = "maheshg98/trivia_backend:${BUILD_NUMBER}"
        CONTNRNAME = 'trivia_backend'
    }

    stages {

        stage('checkout-code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/gavademahesh98/Trivia_Backend_Services.git'
            }
        }

        stage('build-image') {
            steps {
                sh "docker build -t ${DOCKERIMG} ."
            }
        }

        stage('Push-img-repository') {
            steps {
                sh """
                    docker login -u maheshg98 -p Mahesh@8798
                    docker push ${DOCKERIMG}
                """
            }
        }

        stage('Deployment') {
            steps {
                sh """
                    docker kill ${CONTNRNAME} || true
                    docker rm ${CONTNRNAME} || true
                    docker run -itd -p 3000:3000 --name ${CONTNRNAME} ${DOCKERIMG}
                """
            }
        }
    }
}