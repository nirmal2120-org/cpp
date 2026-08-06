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

        stage('Verify Tools') {
            steps {
                sh '''
                    java -version
                    python3 --version
                    pip3 --version
                '''
            }
        }

        stage('Install C++ Scanner') {
            steps {
                sh '''
                    pip3 install --upgrade blackduck-c-cpp
                '''
            }
        }

        stage('Black Duck C/C++ Scan') {
            steps {
                dir('data_structures-II/06-BST') {
                    sh '''
                        PROJECT_DIR=$(pwd)

                        blackduck-c-cpp \
                          -bc "make" \
                          -d "$PROJECT_DIR" \
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
