pipeline {
    agent any

    environment {
        BLACKDUCK_URL = 'https://expressonboarding.app.blackduck.com'
        BLACKDUCK_TOKEN = credentials('blackduck-token')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Pip') {
            steps {
                sh '''
                    sudo apt update || true
                    sudo apt install -y python3-pip || true

                    python3 --version
                    python3 -m pip --version
                '''
            }
        }

        stage('Install Black Duck C++ Tool') {
            steps {
                sh '''
                    python3 -m pip install --upgrade blackduck-c-cpp
                '''
            }
        }

        stage('Black Duck Scan') {
            steps {
                dir('data_structures-II/06-BST') {
                    sh '''
                        blackduck-c-cpp \
                          -bc "make" \
                          -d "$(pwd)" \
                          -proj "Nirmal-Jenkins-cpp" \
                          -vers "cpp" \
                          -bd "$BLACKDUCK_URL" \
                          -a "$BLACKDUCK_TOKEN"
                    '''
                }
            }
        }
    }
}
