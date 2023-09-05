pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: node
    command:
    - sleep
    args:
    - infinity
'''
            defaultContainer 'shell'
        }
    }
    environment {
        POSTMAN_ENVIRONMENT_ID = ""
        POSTMAN_COLLECTION_ID = "16952209-dbf54a1b-b0c0-4d18-b06f-f59b54831a79"
    }
    stages {
        stage('Install Postman CLI') {
            steps {
                sh 'curl -LJ0 "https://github.com/kevinswiber/postman2openapi/releases/download/1.0.0/postman2openapi-1.0.0-x86_64-unknown-linux-musl.tar.gz --output linux.tar.gz'
                sh 'gunzip ./linux.tar.gz'
                sh 'tar -xvf linux.tar'
                sh 'mv postman2openapi-1.0.0-x86_64-unknown-linux-musl/postman2openapi .'
                sh 'postman2openapi'
            }
        }
        stage('Postman CLI Login') {
            steps {
                withCredentials([string(credentialsId: 'PM_CONTRACT_TESTS_VIA_GITHUB_API_KEY', variable: 'POSTMAN_API_KEY')]) {
                    sh 'postman login --with-api-key "${POSTMAN_API_KEY}"'
                }
            }
        }
        /*
        stage('Running collection') {
            steps {
                sh 'postman collection run ${POSTMAN_COLLECTION_ID} --integration-id "145246-${JOB_NAME}${BUILD_NUMBER}" --color off'
            }
        }
        */
    }
}
