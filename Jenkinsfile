piprline{

    agent any

    stages{

        stage('checkout-code'){
            steps{
            git branch: 'main',
            url: 'https://github.com/gavademahesh98/Trivia_Backend_Services.git'
            }
        }

        stage('build-image'){
            steps{
            sh "docker build -t maheshg98/Trivia_backend:${BUILD_NUMBER}"
            }
        }

        stage('Push-img-repository'){
            steps{

            sh """
                    docker login -u maheshg98 -p Mahesh@8798
                    docker push maheshg98/Trivia_backend:${BUILD_NUMBER}

            """
            }
            }

            stage('Deployment'){
                steps{

                        sh  """
                        docker kill Trivia_backend || true
                        docker rm Trivia_backend || true
                        docker run -itd -p 3000:3000 --name Trivia_backend maheshg98/Trivia_backend:${BUILD_NUMBER}

                    """
                }
            }
        }

    }
}