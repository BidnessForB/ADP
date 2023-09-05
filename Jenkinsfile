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
                sh 'curl -o- "https://raw.githubusercontent.com/fyvekatz/portfolio/master/postman_cli_silent.sh" | sh'
            }
        }
        stage('Postman CLI Login') {
            steps {
                withCredentials([string(credentialsId: 'PM_CONTRACT_TESTS_VIA_GITHUB_API_KEY', variable: 'POSTMAN_API_KEY')]) {
                    sh 'postman login --with-api-key "${POSTMAN_API_KEY}"'
                }
            }
        }
        stage('Running collection') {
            steps {
                sh 'postman collection run ${POSTMAN_COLLECTION_ID} --integration-id "145246-${JOB_NAME}${BUILD_NUMBER}" --color off'
            }
        }
    }
}
