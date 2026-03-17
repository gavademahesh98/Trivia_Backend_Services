pipeline {

    agent any

    environment {
        DOCKERIMG = "maheshg98/trivia_backend:${BUILD_NUMBER}"
        CONTNRNAME = 'trivia_backend'
        NETWORK = "trivia-network"
        VOLUME = "monodb_data_trivia"
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
                    # Starting MONOGO DB if it is already present then createing new Container
                    docker start mongodb || docker run -d \
                     --name mongodb \
                     --network ${NETWORK} \
                     -p 27017:27017 \
                     -v ${VOLUME}:/data/db \
                     --restart unless-stopped \
                      mongo

                     sleep 10

                    docker kill ${CONTNRNAME} || true
                    docker rm ${CONTNRNAME} || true
                    docker run -d \
                    -p 3000:3000 \
                    --name ${CONTNRNAME} \
                    --network ${NETWORK} \
                    --restart unless-stopped
                    ${DOCKERIMG}
                """
            }
        }
    }
}