pipeline {
    agent any
    stages {
        stage('Build events-lib') {
            steps {
                build job: 'events-lib', wait: true
            }
        }
        stage('Build User-Micorservice') {
            steps {
                build job: 'User-Micorservice', wait: true, parameters: [
                    string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                ]
            }
        }
        stage('Build authentication-microservice') {
            steps {
                build job: 'Authentication-Microservice', wait: true, parameters: [
                    string(name: 'BUILD_LIB_SUCCESS', value: 'true')
                ]
            }
        }
    }
}
