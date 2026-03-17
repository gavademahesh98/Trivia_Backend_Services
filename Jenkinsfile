pipeline {

    agent any

    environment {
        DOCKERIMG = "maheshg98/trivia_backend:${BUILD_NUMBER}"
        CONTNRNAME = 'trivia_backend'
        NETWORK = "trivia-network"
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
                    # Starting MONOGO DB if it is already present thencreateing new Container
                     docker start mongodb || docker run -d  --name mongodb -p 27017:27017  --network ${NETWORK} mongo

                     sleep 10

                    docker kill ${CONTNRNAME} || true
                    docker rm ${CONTNRNAME} || true
                    docker run -itd -p 3000:3000 --name ${CONTNRNAME} --network ${NETWORK} ${DOCKERIMG}
                """
            }
        }
    }
}