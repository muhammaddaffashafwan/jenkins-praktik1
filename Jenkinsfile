pipeline {
    agent any

    environment {
        VENV_DIR = ".venv"
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    sh '''
                    bash -c "
                        python3 -m venv ${VENV_DIR}
                        source ${VENV_DIR}/bin/activate
                        ${VENV_DIR}/bin/pip install --upgrade pip
                        ${VENV_DIR}/bin/pip install -r requirements.txt
                    "
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh '''
                    bash -c "
                        source ${VENV_DIR}/bin/activate
                        ${VENV_DIR}/bin/python -m pytest || exit 1
                    "
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying...'
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]

                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1369979563513352313/CWu5RVfuo9cxZRv72gHWnq8o5akFYZ1XJAunVpt2ZNTHRp88HzNTZZJYLZWuPDX2CGMV'
                )
            }
        }

        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]

                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1369979563513352313/CWu5RVfuo9cxZRv72gHWnq8o5akFYZ1XJAunVpt2ZNTHRp88HzNTZZJYLZWuPDX2CGMV'
                )
            }
        }
    }
}
