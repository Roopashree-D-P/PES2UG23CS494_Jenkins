pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh '''
                docker build -t backend-app backend
                '''
            }
        }

        stage('Create Network') {
            steps {
                sh '''
                docker network create app-network || true
                '''
            }
        }

        stage('Deploy Backends') {
            steps {
                sh '''
                docker rm -f backend1 backend2 || true

                docker run -d --name backend1 --network app-network backend-app
                docker run -d --name backend2 --network app-network backend-app
                '''
            }
        }

        stage('Deploy NGINX') {
    steps {
        sh '''
        docker rm -f nginx-lb || true

        docker run -d --name nginx-lb \
          --network app-network \
          -p 8081:80 \
          -v $WORKSPACE/nginx/default.conf:/etc/nginx/conf.d/default.conf \
          nginx
        '''
    }
}
            }
        }
    }
}
