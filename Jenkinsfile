pipeline {
    agent any
    
    stages {

        stage('Build') {
            steps {
                script {
                    sh 'apt-get update'
                    sh 'apt-get upgrade -y'

                    sh '''
                       
                        docker compose version
                        '''
                    
                    // Build Docker images
                    sh '''
                        docker compose build --no-cache
                    '''

                    sh '''
                        docker compose down
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Rebuild and start containers
                    sh '''
                        docker compose up -d --build
                    '''
                    
                    // Optionally, check the status of running containers
                    sh '''
                        ps
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                script {

                    sh 'apt-get update'
                    sh 'apt-get upgrade -y'
                    sh '''
                        docker compose version
                        docker compose up -d
                        '''

                    sh 'apt-get install -y python3 python3-venv python3-pip'

                    sh '''
                        python3 -m venv .venv
                        . .venv/bin/activate
                        pip install pytest selenium

                        sleep 15
                        python test_devopstest.py
                        '''
                }
            }
        }
    }
}
